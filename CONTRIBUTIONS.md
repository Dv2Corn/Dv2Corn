# Accepted upstream fixes

Bug fixes merged into other open-source VPN and proxy projects. Each one came with a reproduction and a test that fails before the fix.

| Project | Fix |
|---|---|
| [SagerNet/sing-tun](https://github.com/SagerNet/sing-tun) | [#106](https://github.com/SagerNet/sing-tun/pull/106) Fix IP headers of synthesized UDP replies (stale ECN marks, RFC 3168) · [#107](https://github.com/SagerNet/sing-tun/pull/107) Do not route replies of peer-initiated UDP flows into the TUN |
| [SagerNet/sing](https://github.com/SagerNet/sing) | [#119](https://github.com/SagerNet/sing/pull/119) Fix a race between Read and Close in cached connections · [#118](https://github.com/SagerNet/sing/pull/118) Cancel the winning attempt's context in parallel dial |
| [2dust/v2rayN](https://github.com/2dust/v2rayN) | [#10548](https://github.com/2dust/v2rayN/pull/10548) Keep the WebSocket path query when removing `ed` (sing-box) |
| [2dust/v2rayNG](https://github.com/2dust/v2rayNG) | [#6413](https://github.com/2dust/v2rayNG/pull/6413) Split share link query pairs on the first `=` only |
| [MHSanaei/3x-ui](https://github.com/MHSanaei/3x-ui) | [#7042](https://github.com/MHSanaei/3x-ui/pull/7042) A partial client update no longer wipes fields it does not mention |
| [alireza0/s-ui](https://github.com/alireza0/s-ui) | [#1343](https://github.com/alireza0/s-ui/pull/1343) Accept TLS handshakes without SNI when the panel domain is an IP |
| [throneproj/Throne](https://github.com/throneproj/Throne) | [#2098](https://github.com/throneproj/Throne/pull/2098) Apply a group's front and landing proxy to routing rule outbounds |
| [GUI-for-Cores/GUI.for.SingBox](https://github.com/GUI-for-Cores/GUI.for.SingBox) | [#39](https://github.com/GUI-for-Cores/GUI.for.SingBox/pull/39) Keep colons in inbound user passwords · [#40](https://github.com/GUI-for-Cores/GUI.for.SingBox/pull/40) Keep rules and rule-sets saved without `action` or `type` · [#41](https://github.com/GUI-for-Cores/GUI.for.SingBox/pull/41) System proxy with an IPv6 listen address |
| [GUI-for-Cores/GUI.for.Clash](https://github.com/GUI-for-Cores/GUI.for.Clash) | [#7](https://github.com/GUI-for-Cores/GUI.for.Clash/pull/7) Keep commas inside rules when splitting them · [#8](https://github.com/GUI-for-Cores/GUI.for.Clash/pull/8) Keep list values in `nameserver-policy` |
| [Zephyruso/zashboard](https://github.com/Zephyruso/zashboard) | [#822](https://github.com/Zephyruso/zashboard/pull/822) Show the full day count for uptime over 30 days · [#823](https://github.com/Zephyruso/zashboard/pull/823) Bracket bare IPv6 backend hosts in URLs |
| [MetaCubeX/mihomo](https://github.com/MetaCubeX/mihomo) | [#3415](https://github.com/MetaCubeX/mihomo/pull/3415) Convert HTTPUpgrade share links correctly |
| [MetaCubeX/metacubexd](https://github.com/MetaCubeX/metacubexd) | [#2246](https://github.com/MetaCubeX/metacubexd/pull/2246) Keep config edits on remote profiles across refresh · [#2247](https://github.com/MetaCubeX/metacubexd/pull/2247) Offer only TUN stacks mihomo accepts · [#2250](https://github.com/MetaCubeX/metacubexd/pull/2250) Keep the most recent closed connections |
| [hiddify/hiddify-app](https://github.com/hiddify/hiddify-app) | [#2465](https://github.com/hiddify/hiddify-app/pull/2465) Accept a base64 profile title without padding |
| [hiddify/hiddifypanel](https://github.com/hiddify/hiddifypanel) | [#176](https://github.com/hiddify/hiddifypanel/pull/176) Turning off HTTP no longer drops Shadowsocks 2022 and Mieru · [#178](https://github.com/hiddify/hiddifypanel/pull/178) Renew no longer exceeds max active users · [#199](https://github.com/hiddify/hiddifypanel/pull/199) Settings longer than 3072 characters save on MariaDB |
| [pymumu/smartdns](https://github.com/pymumu/smartdns) | [#2497](https://github.com/pymumu/smartdns/pull/2497) Keep HTTP/3 for `server-http3` with an `https://` URL |
