# Player Information

## Endpoints

| Method | Path |
|--------|------|
| GET | `https://services.facepunch.com/sbox/player/{steamId}` |
| GET | `https://services.facepunch.com/sbox/player/{steamId}/overview` |
| GET | `https://services.facepunch.com/sbox/player/{steamId}/achievementprogress` |

## Description
Retrieve information about a specific S&box player by Steam ID — basic profile, aggregate platform overview, and per-package achievement progress.

## Path Parameters

| Name    | Type   | Required | Description |
|---------|--------|----------|-------------|
| steamId | long   | Yes      | Player's Steam ID (64-bit, 17-digit format) |

## Query Parameters

| Endpoint | Name | Type | Required | Default | Description |
|----------|------|------|----------|---------|-------------|
| `/achievementprogress` | take | int | No | 10 | Maximum number of packages to return |

---

## `GET /sbox/player/{steamId}`

### Example Request
`GET /sbox/player/76561197960279927`

### Example Response
```json
{
  "Id": 76561197960279927,
  "Name": "garry",
  "Url": "/u/garry",
  "Avatar": "https://avatars.steamstatic.com/adfd2545a3a5f0eb07ad042873be6aa4e03ef660_medium.jpg",
  "Online": true,
  "Score": 1375
}
```

### Response Fields
| Field   | Type    | Description |
|---------|---------|-------------|
| Id      | long    | Player's Steam ID (64-bit) |
| Name    | string  | Display name |
| Url     | string  | Relative URL to the player profile (`/u/...`) |
| Avatar  | string  | URL to the Steam avatar image |
| Online  | bool    | Indicates if the player is currently active in S&box (omitted when false in some responses) |
| Private | bool    | Indicates a private profile (omitted when false) |
| Score   | int     | Player's reputation/activity score |

---

## `GET /sbox/player/{steamId}/overview`

Aggregated platform stats and recent activity for the player.

### Example Response (truncated)
```json
{
  "Player": { "Id": 76561197960279927, "Name": "garry", "Score": 1375 },
  "GamesPlayed": 341,
  "TotalSessions": 3028,
  "SecondsPlayed": 548335,
  "Achievements": 60,
  "TotalFavourites": 68,
  "TotalReviews": 33,
  "NegativeReviews": 3,
  "PositiveReviews": 15,
  "Avatar": "{\"Items\":[...],\"Height\":0.62,\"DisplayName\":\"Garry\"}",
  "LatestReviews": [ /* PackageReviewDto[] */ ],
  "MostPlayed": { /* PackageWrapMinimal */ },
  "LatestPlayed": { /* PackageWrapMinimal */ }
}
```

### Response Fields (`PlayerOverview`)
| Field           | Type    | Description |
|-----------------|---------|-------------|
| Player          | object  | Basic player profile (same shape as the root endpoint) |
| GamesPlayed     | long    | Distinct games the player has launched |
| TotalSessions   | long    | Total play sessions across all packages |
| SecondsPlayed   | long    | Cumulative time played, in seconds |
| Achievements    | long    | Number of achievements unlocked |
| TotalFavourites | long    | Packages this player has favourited |
| TotalReviews    | long    | Reviews authored by this player |
| NegativeReviews | long    | Subset of reviews scored as Negative |
| PositiveReviews | long    | Subset of reviews scored as Positive |
| Avatar          | string  | JSON-encoded Citizen avatar configuration (clothing items, height, name…) — parse client-side |
| LatestReviews   | array   | Most recent reviews by this player (`PackageReviewDto[]`) |
| MostPlayed      | object  | Package with the highest play time (`PackageWrapMinimal`) |
| LatestPlayed    | object  | Most recently played package (`PackageWrapMinimal`) |
| TopPlayed       | array   | Top played packages with `SecondsPlayed`, `AchUnlocked`, `LastSeen` |
| RecentlyPlayed  | array   | Most recently played packages (same shape as `TopPlayed`) |

> The `Avatar` field is a **string** containing JSON. It encodes the player's Citizen outfit; you must `JSON.parse` it before reading nested fields.

---

## `GET /sbox/player/{steamId}/achievementprogress`

Per-package achievement progress for the player.

### Example Response (truncated)
```json
[
  {
    "Package": { /* PackageWrapMinimal */ },
    "Achievements": [ /* AchievementDto[] */ ],
    "LastSeen": "2026-05-18T19:12:57.7+00:00",
    "Unlocked": 3,
    "Score": 80,
    "Total": 12,
    "TotalScore": 500
  }
]
```

### Response Fields (`PlayerAchievementProgress`)
| Field        | Type   | Description |
|--------------|--------|-------------|
| Package      | object | Minimal package wrapper |
| Achievements | array  | Per-achievement entries (see Achievements endpoint) |
| LastSeen     | string | Last play timestamp for this package (ISO 8601) |
| Unlocked     | int    | Number of achievements the player has unlocked |
| Score        | int    | Sum of `Score` from unlocked achievements |
| Total        | int    | Total achievements in the package |
| TotalScore   | int    | Sum of `Score` across all achievements in the package |

## Error Responses

| Status Code | Description |
|-------------|-------------|
| 404 | Player not found |
| 400 | Invalid Steam ID format |
| 429 | Rate limit exceeded |

## Notes

- Steam IDs must be 64-bit (17-digit, prefixed with `7656`).
- The `Url` field is a relative path: prepend `https://sbox.game` to get the full URL.
- The `Online` and `Private` fields are omitted from responses when false.
- Use the `take` parameter on `/achievementprogress` to limit how many packages are returned (default 10).
