# Third-party notices

Traffic Killer itself is released under the [MIT License](LICENSE).

## Windows builds

The Windows `.exe` files are self-contained: they bundle the .NET 8 runtime, WPF and Windows Forms.

| Component | Copyright | License |
|---|---|---|
| .NET Runtime 8 | © .NET Foundation and Contributors | [MIT](https://github.com/dotnet/runtime/blob/main/LICENSE.TXT) |
| WPF | © .NET Foundation and Contributors | [MIT](https://github.com/dotnet/wpf/blob/main/LICENSE.TXT) |
| Windows Forms | © .NET Foundation and Contributors | [MIT](https://github.com/dotnet/winforms/blob/main/LICENSE.TXT) |

The .NET runtime also contains components under their own permissive licenses, listed in
[THIRD-PARTY-NOTICES.TXT of dotnet/runtime](https://github.com/dotnet/runtime/blob/main/THIRD-PARTY-NOTICES.TXT).

## macOS builds

The macOS app uses only system frameworks (AppKit, SwiftUI, Network, SystemConfiguration) and SF Symbols
provided by macOS at runtime; nothing third-party is bundled.

## Sounds

The notification sounds are synthesized by the project itself and are covered by the MIT License.

## Online services

The app queries these public services; their own terms apply:

| Purpose | Services | What is sent |
|---|---|---|
| Public IP check | api.ipify.org, ipv4.icanhazip.com, ifconfig.me | a plain request (the service sees the IP it comes from) |
| VPN location | ipwho.is; fallbacks: ipapi.co (macOS), get.geojs.io | the VPN exit IP — only while the VPN is connected |
