# Trust Tunnel Export

This directory contains ready-to-paste exports for the iOS Trust Tunnel app.

Files:
- `vpn.txt`: merged `T + A` proxy list, expanded from:
  - your current Trust Tunnel VPN list
  - `rule-set/manual-t.json`
  - `rule-set/manual-a.json`
  - router client lists `c-T-*` and `c-A-*`
  - explicit IP/domain fallback rules from `openwrt-xray-router/xray/50-routing.json.tpl`
  - flattened upstream `geosite` tags used by the current Throne profile:
    - `openai`, `anthropic`, `category-ai-!cn`, `youtube`, `spotify`, `facebook`, `instagram`, `whatsapp`, `twitter`, `telegram`, `tiktok`, `discord`, `linkedin`, `microsoft`, `google`, `wikimedia`, `bbc`, `cnn`, `netflix`, `kinopub`
  - IPv4 CIDR entries broader than `/24` are intentionally dropped from this export.
- `bypass.txt`: direct list for Trust Tunnel `Bypass`, built from:
  - `rule-set/manual-d.json`
  - router client direct list `openwrt-xray-router/lists/c-D-domains.txt`
- `excluded-routes.txt`: IP/CIDR list for Trust Tunnel `Excluded routes`.
  - This approximates `geoip:private` plus local link-local/multicast discovery ranges needed for Apple device-to-device features.
- `unsupported-patterns.txt`: `keyword` / `regex` items that current Trust Tunnel syntax does not express directly.
  - Some of them are already covered by explicit domains in `vpn.txt` or `bypass.txt`, but this file shows what could not be losslessly translated.
- `stats.txt`: quick counts for the generated export.

Trust Tunnel mapping:
- `Vpn` -> `vpn.txt`
- `Bypass` -> `bypass.txt`
- `Excluded routes` -> `excluded-routes.txt`

Important:
- Trust Tunnel plain `example.com` rules do not cover arbitrary subdomains. Because of that, this export adds wildcard companions like `*.example.com` where coverage matters.
- Your previous Trust Tunnel VPN list was preserved and re-included before expansion.
- The current export keeps exact IPs and CIDR prefixes `/24` and narrower in `vpn.txt`.
- For Telegram and WhatsApp IPv6 fallback, `vpn.txt` also includes OpenCCK `cidr6` prefixes filtered to `/62` and narrower, plus uncovered exact `ip6` addresses.
