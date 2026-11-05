# Privacy

**English** · [Русский](PRIVACY.ru.md)

Traffic Killer has no accounts, no analytics and no crash reporting, and it has no server of its own.
This page lists every connection the app makes. The source code is not public yet, so for now you can check
this list with a network monitor.

## What goes over the network

| Purpose | Who receives it | What they see | When | Can it be turned off? |
|---|---|---|---|---|
| Public IP check | `api.ipify.org`; if that fails, `ipv4.icanhazip.com`, then `ifconfig.me` | The IP you connect from, and the time | Every 30 seconds by default, whether or not the VPN is connected | No, only made less frequent (up to every 10 minutes) in Settings → VPN & IP |
| VPN location | `ipwho.is`; if that fails, `get.geojs.io` (and `ipapi.co` on macOS) | The VPN exit IP, in the request and as the sender | Once per exit IP, only while the VPN is connected and healthy | Yes: Show the VPN location |
| Update check | `api.github.com` | The VPN exit IP, as the sender | At most once a day, only while the VPN is connected and healthy | Yes: Check for updates |

**The IP check sees your real IP while the VPN is off.** This is unavoidable: comparing the public IP with
your real one is how the app notices traffic going around the tunnel. While the VPN is off, these services
receive the same address as any site you open. On macOS, strict mode blocks these requests too.

All requests use HTTPS and send only the generic user agent `curl/8`, without the app's name. Their DNS
lookups go through your system's resolver.

The app also runs local system tools (`scutil`, `lsof`, `pfctl` and others on macOS). They send nothing.

## What stays on your computer

| File | Contains |
|---|---|
| `config.json` | Settings, the list of protected apps, your real IP addresses, and VPN server addresses for strict mode |
| `incidents.json` | Failure history: times, reasons, closed apps, packet counts and the risk level |
| `guard.log` | Events. IP addresses are shortened to `203.0.x.x`; logs from before version 1.3 may hold full addresses |
| `/Library/Application Support/Traffic Killer/` (macOS strict mode) | The helper's settings, status and log, including VPN server addresses |

The files are in `~/Library/Application Support/Traffic Killer/` on macOS and `%LOCALAPPDATA%\Traffic Killer\` on Windows. Nothing is sent anywhere automatically. Before attaching
a log to a bug report, read it: it can still name your apps and VPN.

Notifications about a remembered real IP show the address on your screen.

## Updates

The app only compares version numbers and shows a link to the releases page. It downloads and installs nothing.
