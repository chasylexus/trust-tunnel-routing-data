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
  - Router-authoritative Telegram and WhatsApp fallback ranges are preserved even when they are broader than `/24`, so the export matches the live router behavior more closely.
  - The VPN IP section now follows a shared OpenCCK-derived fallback set for Telegram, WhatsApp, X, TikTok, Instagram, and 4PDA: exact IPs are preserved, and CIDRs are kept only when they stay within the common safety envelope (`IPv4 >= /17`, `IPv6 >= /32`).
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
- The current export keeps router-authoritative client destination IPs/CIDRs in `vpn.txt`.
- Telegram and WhatsApp IPv6 fallback in `vpn.txt` now comes from the router's explicit CIDR rules rather than a large auto-generated exact-IP import.
