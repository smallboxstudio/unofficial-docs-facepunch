# Package Stats

## Endpoints

| Method | Path |
|--------|------|
| GET | `https://services.facepunch.com/sbox/package/stats/2/{packageIdent}` |
| GET | `https://services.facepunch.com/sbox/package/stats/2/{packageIdent}/u/{steamId}` |

## Description
Retrieve aggregated gameplay stats published by a package. The first endpoint returns the global aggregate for every stat the package declares. The second returns a single user's per-stat values.

## Path Parameters

| Name         | Type   | Required | Description |
|--------------|--------|----------|-------------|
| packageIdent | string | Yes      | Full package identifier (`org.package`) |
| steamId      | long   | Yes (user) | Steam ID of the user whose stats to retrieve |

---

## Example Requests
```
GET /sbox/package/stats/2/facepunch.testbed
GET /sbox/package/stats/2/facepunch.testbed/u/76561197960279927
```

---

## Example Response (global)
```json
[
  {
    "Name": "balls_fired",
    "Title": "balls_fired",
    "Description": "",
    "Unit": "",
    "Value": 1,
    "ValueString": "1",
    "Players": 8109,
    "Max": 1,
    "Avg": 1,
    "Min": 1,
    "Sum": 8897209
  }
]
```

## Response Fields

### `GlobalStat`
| Field        | Type   | Description |
|--------------|--------|-------------|
| Name         | string | Stat identifier |
| Title        | string | Display title |
| Description  | string | Optional description |
| Unit         | string | Unit of measure (e.g. "kills", "seconds") |
| Velocity     | number | Rate of change indicator |
| Value        | number | Reference value |
| ValueString  | string | Pre-formatted display value |
| Players      | int    | Number of players who contributed to the stat |
| Max          | number | Highest individual value |
| Avg          | number | Mean value |
| Min          | number | Lowest individual value |
| Sum          | number | Sum across all players |

### `PlayerStat` (per-user endpoint)
| Field        | Type   | Description |
|--------------|--------|-------------|
| Name         | string | Stat identifier |
| Title        | string | Display title |
| Description  | string | Optional description |
| Unit         | string | Unit of measure |
| Value        | number | Player's current value |
| ValueString  | string | Pre-formatted display value |
| Max / Min / Avg / Sum | number | Per-player aggregates |
| First        | string | Timestamp of the first contribution (ISO 8601) |
| FirstValue   | number | Value at first contribution |
| Last         | string | Timestamp of the most recent contribution (ISO 8601) |
| LastValue    | number | Value at most recent contribution |

## Error Responses

| Status Code | Description |
|-------------|-------------|
| 404 | Package or player not found |
| 400 | Invalid identifier |
| 429 | Rate limit exceeded |

## Notes

- Returns an empty array when the package declares no stats or the user has none recorded.
- `Velocity` is populated for global stats only.
- Stat names are arbitrary strings defined by each package.
