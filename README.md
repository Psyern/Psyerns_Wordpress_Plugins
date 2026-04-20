# Psyerns WordPress Plugins

Suite of WordPress plugins for DayZ server communities built around the
[Psyerns_Framework](https://github.com/Psyern/Psyerns-Framework) DayZ mod.

Each plugin lives in its own repository with its own version history, issue
tracker, and release cycle. This repository is a landing-page / discovery
index — it contains no plugin code.

---

## Requirements

The WordPress plugins in this suite are **data consumers**. They do not
read DayZ server state directly — all data (listings, transactions,
leaderboards, balances, etc.) is transmitted **from the DayZ server into
WordPress** via HTTPS.

To enable that data flow, the DayZ server must run the companion mod
[Psyerns_Framework](https://github.com/Psyern/Psyerns-Framework), which
ships the shared REST client (`PF_WebClient`) and per-feature integration
modules (`PF_AH_Sync` for AuctionHouse, `PF_LeaderboardExport` for the
Leaderboard, etc.).

Without the Psyerns_Framework mod installed and configured on the DayZ
server, the WordPress plugins will show empty states — they have no
other way to receive data.

**Authentication.** The DayZ server holds a single API key in its
Psyerns_Framework mod configuration. Every WordPress plugin that should
receive data from that server must have the **same API key** entered in
its own admin settings — each plugin validates incoming requests against
it independently. If you rotate the key on the mod side, update all WP
plugins accordingly.

---

## Plugins

| Plugin | Slug | Version | Status | Repository |
|---|---|---|---|---|
| Psyerns Framework | `psyerns-framework` | 1.0.0 | Stable | _coming soon_ |
| Psyerns AuctionHouse | `psyerns-auctionhouse` | 1.0.0 | Initial Release | [Psyerns_AuctionHouse_Plugin](https://github.com/Psyern/Psyerns_AuctionHouse_Plugin) |
| Psyerns Leaderboard | `psyerns-leaderboard` | 1.0.0 | Initial Release | [Psyerns_Leaderboard_Plugin](https://github.com/Psyern/Psyerns_Leaderboard_Plugin) |

---

## Architecture

```
   ┌──────────────────────────────────────────┐
   │          DayZ Server                     │
   │  ┌────────────────────────────────────┐  │
   │  │   Psyerns_Framework (DayZ Mod)     │  │
   │  │   • REST client (PF_WebClient)     │  │
   │  │   • Integration modules:           │  │
   │  │     - PF_LeaderboardExport         │  │
   │  │     - PF_AH_Sync (AuctionHouse)    │  │
   │  │     - PF_ServerStatus              │  │
   │  │     - etc.                         │  │
   │  └──────────────┬─────────────────────┘  │
   └─────────────────┼────────────────────────┘
                     │  HTTPS + API-Key
                     ▼
   ┌──────────────────────────────────────────┐
   │          WordPress Server                │
   │  ┌────────────────────────────────────┐  │
   │  │  Psyerns Framework (WP Plugin)     │  │
   │  │  • Leaderboard, Whitelist, etc.    │  │
   │  │  • Shared theme system (9 themes)  │  │
   │  └────────────────────────────────────┘  │
   │                                          │
   │  ┌────────────────────────────────────┐  │
   │  │  Psyerns AuctionHouse (WP Plugin)  │  │
   │  │  • Marketplace + Steam login       │  │
   │  │  • Buy-Now + Bidding from web      │  │
   │  │  • Price history charts            │  │
   │  │  • Reuses Framework themes         │  │
   │  └────────────────────────────────────┘  │
   └──────────────────────────────────────────┘
```

**Plugin relationships.** The Framework plugin is the visual foundation —
it ships the nine CSS themes (`stalker`, `cyberpunk`, `frostbite`, etc.)
and a common base stylesheet. AuctionHouse detects the Framework plugin at
runtime (soft-dependency) and reuses its themes when available; otherwise
it falls back to its own built-in styling. Each plugin remains independently
installable.

On the DayZ-mod side, `Psyerns_Framework` provides the shared REST client
and is extended by per-plugin integration modules (`PF_AH_Sync` for
AuctionHouse, `PF_LeaderboardExport` for the Leaderboard, etc.).

---

## Contributing

Issues and pull requests belong in the dedicated plugin repos, not this
index. Each plugin has its own `CONTRIBUTING.md` (or will, as it is
published).

## License

All plugins in this suite are MIT-licensed.
