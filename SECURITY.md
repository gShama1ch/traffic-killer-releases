# Security

**English** · [Русский](SECURITY.ru.md)

## What protects you, and what does not

| Situation | Normal mode | macOS strict mode | Windows firewall layer |
|---|---|---|---|
| VPN drops, listed app is running | App is frozen and closed within milliseconds of detection. What it sent before detection may have gone out | Packets are dropped by the kernel | Outbound traffic of listed apps is blocked for the duration of the failure, right after detection |
| VPN drops, app is not on the list | Not protected | TCP/UDP of your macOS user is blocked | Not protected |
| Traffic from system services running as root | Not protected | Not covered: the rule applies to your user only | Not protected |
| DNS lookups | Warning or failure when servers are outside the tunnel | Not covered: they go through a system service | Warning or failure |
| ICMP and other non-TCP/UDP traffic | Not protected | Not covered | Not protected |
| WebRTC inside a browser | Not checked | Not checked | Not checked |
| The first moments after boot, before the app or helper starts | Not protected | Not protected until the helper loads its rules | Not protected |

Traffic Killer does not hide you on its own and does not replace a VPN. It also cannot protect you from
malware already running under your account.

## The IP exposure risk scale

After each failure, the notification and the history show how long the gap was, how many packets went out, and
a four-step scale. It reads the packet counters of the network card and needs no extra permissions.

| Risk | When |
|---|---|
| None | macOS strict mode was active, or no listed app was running |
| Low | Up to 20 packets and up to 250 ms |
| Medium | Up to 200 packets and up to 2 seconds |
| High | More than that, or protection was off or paused |
| Unknown | The gap could not be measured, for example after the computer slept |

**This is a rule of thumb, not a probability.** Even one packet can show a server your IP, so "low" does not mean
"safe", and zero packets outside strict mode is still "low": the counters are read a moment after the network
event. The number covers every program on the card, including the VPN itself, so it is an estimate from above.
It is not how many packets your listed apps sent. The app cannot tell which packets were those without
filtering traffic. In strict mode, the kernel's own counter shows how many packets it dropped; on Windows
nothing is counted for the firewall layer.

## The macOS root helper

Strict mode installs a small script that runs as root, about 175 lines you can read before turning it on:
`Traffic Killer.app/Contents/Resources/strict-helper.zsh`. It only loads firewall rules into its own anchor and writes status files.

- Your password goes to the standard macOS prompt, never to the app.
- The helper and its settings are owned by root, so normal programs cannot change them once installed.
- If the helper crashes, restarts or is updated, the rules stay in place until new ones replace them atomically.
- If rules fail to load, the app shows the error and does not report protection as active.

**Known weakness:** during installation, the files are prepared in a temporary folder owned by your user just
before the password prompt. A malicious program already running under your account could swap them, and they
would then run as root. A signed helper will remove this; it requires a signing decision that has not been made yet.

### Removing it

Use **Settings → VPN & IP** to remove the helper **before** deleting the app. If you delete the app first,
the helper keeps blocking traffic outside the VPN. To remove it by hand:

```bash
sudo launchctl bootout system/com.traffickiller.strict
```

```bash
sudo pfctl -a com.apple/250.TrafficKiller -F all
```

```bash
sudo rm -f /Library/PrivilegedHelperTools/com.traffickiller.strict /Library/LaunchDaemons/com.traffickiller.strict.plist
```

## Verifying downloads

**Builds are not yet signed by Apple or Microsoft**, which is why the system warns you on first launch. Each
release publishes `SHA256SUMS.txt`:

```bash
shasum -a 256 -c SHA256SUMS.txt
```

This detects a damaged download. It does not prove who made the file, because the checksums come from the
same place as the builds.

Every build is automatically checked for personal data, and a build that fails stops.

## Reporting a vulnerability

Please do not describe a vulnerability in a public issue. Use **Security → Report a vulnerability** in the
[releases repository](https://github.com/gShama1ch/traffic-killer-releases/security). This is a one-person
project: expect a reply within about a week, with no bounty.

Most useful in a report: the version, the operating system, the VPN client, and the steps to reproduce.
