# Achievements

## Endpoint
`GET https://services.facepunch.com/sbox/achievement/list?package={packageIdent}`

## Description
List every achievement declared by a S&box package, including unlock conditions, global unlock fractions, icons, and scoring weights.

## Query Parameters

| Name    | Type   | Required | Description |
|---------|--------|----------|-------------|
| package | string | Yes      | Full package identifier (`org.package`) |

---

## Example Request
`GET /sbox/achievement/list?package=facepunch.testbed`

---

## Example Response
```json
[
  {
    "Name": "100_cubes",
    "Title": "100 Cubes",
    "Description": "You have shot 100 cubes",
    "Icon": "https://cdn.sbox.game/upload/i/a4db95ca/a79a/46f8/a87c/fa37c3516a71.png",
    "SourceStat": "cubes_fired",
    "Max": 100,
    "ShowProgress": true,
    "UnlockMode": 1,
    "Score": 50,
    "GlobalUnlocks": 251,
    "GlobalFraction": 0.0118
  }
]
```

## Response Fields (`AchievementDto`)

| Field             | Type    | Description |
|-------------------|---------|-------------|
| Name              | string  | Stable internal identifier |
| Title             | string  | Display title |
| Description       | string  | Display description |
| Icon              | string  | CDN URL of the achievement icon |
| Visibility        | enum    | `Visible`, `VisibleWhenUnlocked`, `Hidden` |
| SourceStat        | string  | Stat name driving auto-unlock (when `UnlockMode = Stat`) |
| SourceAggregation | enum    | Aggregation applied to `SourceStat` (`Sum`, `Max`, `Min`, `Last`, `Avg`) |
| Min               | number  | Minimum stat value for progress |
| Max               | number  | Stat value required to unlock |
| ShowProgress      | bool    | Whether the client should show a progress bar |
| UnlockMode        | enum    | `0` = Manual, `1` = Stat |
| Score             | int     | Score awarded when unlocked |
| GlobalUnlocks     | int     | Count of users who have unlocked it globally |
| GlobalFraction    | number  | Fraction of users who have unlocked it (0–1) |
| Unlocked          | string? | If queried in a user context, timestamp of unlock (ISO 8601) — otherwise omitted |

## Error Responses

| Status Code | Description |
|-------------|-------------|
| 404 | Package not found |
| 400 | Missing or invalid `package` parameter |
| 429 | Rate limit exceeded |

## Notes

- Achievements are returned in package-defined order.
- `GlobalUnlocks` and `GlobalFraction` are recomputed periodically; values may lag behind realtime.
- The "unlock" mutation (`POST /sbox/achievement/unlock`) requires authentication and is not part of the public read surface.
