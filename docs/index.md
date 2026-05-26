# Facepunch S&box API Documentation

Welcome to the unofficial documentation of the Facepunch S&box backend API
(base URL: `https://services.facepunch.com/sbox`).

The endpoints below are inferred from public network traffic and from the
public client interfaces in
[Facepunch/sbox-public/engine/Sandbox.Services/Api](https://github.com/Facepunch/sbox-public/tree/master/engine/Sandbox.Services/Api).

## Documented endpoints

| Group | Endpoints |
|-------|-----------|
| [Search](en/search.md) | `GET /package/find/2` |
| [Packages](en/package.md) | `GET /package/get/2/{ident}` |
| [Package Versions](en/versions.md) | `GET /package/versions/2/{ident}` |
| [Reviews](en/reviews.md) | `GET /package/reviews/{ident}` · `GET /package/reviews/{ident}/{steamId}` |
| [Stats](en/stats.md) | `GET /package/stats/2/{ident}` · `GET /package/stats/2/{ident}/u/{steamId}` |
| [Leaderboards](en/leaderboard.md) | `GET /package/leaderboard/2` · `GET /package/leaderboard/1/...` |
| [Achievements](en/achievements.md) | `GET /achievement/list` |
| [Players](en/players.md) | `GET /player/{steamId}` · `/overview` · `/achievementprogress` |
| [News](en/news.md) | `GET /news` · `/news/platform` · `/news/package/{ident}` · `/news/organization/{ident}` |
| [Utility](en/utility.md) | `GET /utility/avatars` |

> ⚠️ **Note**: This documentation is based on publicly accessible endpoints
> and the public Sandbox.Services client. Authenticated mutations
> (uploads, ratings, achievement unlocks, account/login, storage writes,
> notifications) require a Bearer API key and are not covered here.
> Endpoints may change if Facepunch updates their services.
