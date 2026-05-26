# Package Reviews

## Endpoints

| Method | Path |
|--------|------|
| GET | `https://services.facepunch.com/sbox/package/reviews/{packageIdent}` |
| GET | `https://services.facepunch.com/sbox/package/reviews/{packageIdent}/{steamId}` |

## Description
Retrieve user-submitted reviews for a specific S&box package. The list endpoint returns paginated reviews and can be filtered by score and tag flags. The single-review endpoint returns one user's review of the package.

## Path Parameters

| Name          | Type   | Required | Description |
|---------------|--------|----------|-------------|
| packageIdent  | string | Yes      | Full package identifier (format: `org.package`) |
| steamId       | long   | Yes (single) | Steam ID (64-bit) of the reviewer |

## Query Parameters (list endpoint)

| Name      | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| skip      | int  | Yes      | —       | Offset for pagination |
| take      | int  | Yes      | —       | Maximum number of reviews to return |
| score     | int  | No       | 0       | Filter by `ReviewScore` value (0 = all, 1 = Negative, 2 = Positive, 3 = Promise) |
| positives | int  | No       | 0       | Bitmask filter for `ReviewPositiveTags` |
| negatives | int  | No       | 0       | Bitmask filter for `ReviewNegativeTags` |

---

## Example Requests
```
GET /sbox/package/reviews/facepunch.testbed/?skip=0&take=10
GET /sbox/package/reviews/facepunch.sandbox/76561197960279927
```

---

## Example Response (list)
```json
{
  "Count": 139,
  "Skip": 0,
  "Take": 10,
  "Entries": [
    {
      "Player": {
        "Id": 76561199134354514,
        "Name": "Strange™",
        "Url": "/u/76561199134354514",
        "Avatar": "https://avatars.steamstatic.com/b7066fb6ff81e0241bea7620e0bda9e06b669c13_medium.jpg",
        "Online": true,
        "Score": 1455
      },
      "SteamId": 76561199134354514,
      "PackageId": 20223,
      "Content": "Useful for helping me understand physics of certain objects:)",
      "Score": 2,
      "SecondsPlayed": 1784,
      "Created": "2026-05-25T18:18:51.13+00:00",
      "Updated": "2026-05-25T18:18:51.13+00:00",
      "Positives": 4095
    }
  ]
}
```

## Example Response (single review)
```json
{
  "Player": {
    "Id": 76561197960279927,
    "Name": "garry",
    "Url": "/u/garry",
    "Avatar": "https://avatars.steamstatic.com/adfd2545a3a5f0eb07ad042873be6aa4e03ef660_medium.jpg",
    "Online": true,
    "Score": 1375
  },
  "SteamId": 76561197960279927,
  "PackageId": 12,
  "Content": "wwow!",
  "Score": 3,
  "SecondsPlayed": 51871,
  "Created": "2026-04-30T11:06:03.8+01:00",
  "Updated": "2026-05-01T09:52:16.7+00:00",
  "Positives": 2354,
  "Negatives": 2178
}
```

## Response Fields

### List wrapper
| Field   | Type  | Description |
|---------|-------|-------------|
| Count   | int   | Total review count for the package |
| Skip    | int   | Echoed pagination offset |
| Take    | int   | Echoed page size |
| Entries | array | Array of `PackageReviewDto` |

### Review entry (`PackageReviewDto`)
| Field         | Type   | Description |
|---------------|--------|-------------|
| Player        | object | Author profile (see Players endpoint) |
| SteamId       | long   | Reviewer's Steam ID |
| PackageId     | long   | Internal numeric package id |
| Content       | string | Review text (may be empty) |
| Score         | int    | `ReviewScore` enum value: 1 = Negative, 2 = Positive, 3 = Promise |
| SecondsPlayed | int    | Time the reviewer played before posting |
| Created       | string | Creation timestamp (ISO 8601) |
| Updated       | string | Last update timestamp (ISO 8601) |
| Positives     | int    | Bitmask of `ReviewPositiveTags` |
| Negatives     | int    | Bitmask of `ReviewNegativeTags` |

### `ReviewPositiveTags` (bit flags)
| Bit | Value | Name | Icon |
|-----|-------|------|------|
| 0  | 1     | Graphics      | palette |
| 1  | 2     | Audio         | volume_up |
| 2  | 4     | Gameplay      | sports_esports |
| 3  | 8     | Story         | menu_book |
| 4  | 16    | Multiplayer   | groups |
| 5  | 32    | Originality   | lightbulb |
| 6  | 64    | Performance   | speed |
| 7  | 128   | Polish        | auto_awesome |
| 8  | 256   | Addictive     | favorite |
| 9  | 512   | Replayability | replay |
| 10 | 1024  | Controls      | gamepad |
| 11 | 2048  | Updates       | update |

### `ReviewNegativeTags` (bit flags)
| Bit | Value | Name | Icon |
|-----|-------|------|------|
| 1  | 2     | Unfinished    | construction |
| 2  | 4     | Unoptimized   | slow_motion_video |
| 3  | 8     | Bad Controls  | gamepad |
| 4  | 16    | Confusing     | help |
| 5  | 32    | Slop          | mop |
| 6  | 64    | Generated Art | smart_toy |
| 7  | 128   | Pay to Win    | paid |
| 8  | 256   | Stolen        | report |
| 9  | 512   | Errors        | error |
| 10 | 1024  | Load Times    | hourglass_top |
| 11 | 2048  | Buggy         | bug_report |
| 12 | 4096  | Clicker       | touch_app |
| 13 | 8192  | Idle          | autorenew |

## Error Responses

| Status Code | Description |
|-------------|-------------|
| 404 | Package or review not found |
| 400 | Invalid identifier or query parameters |
| 429 | Rate limit exceeded |

## Notes

- `Positives` and `Negatives` are stored as `[Flags]` bitmasks; combine bits to filter on multiple tags.
- The single-review endpoint returns `404` when the user has not reviewed the package.
- Reviews with `DisplayMode = HiddenFromPublic` are filtered out of public responses.
- `Score` follows the `ReviewScore` enum from the engine source: `Negative = 1`, `Positive = 2`, `Promise = 3`.
