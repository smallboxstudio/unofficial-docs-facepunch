# Leaderboards

## Endpoints

| Method | Path |
|--------|------|
| GET | `https://services.facepunch.com/sbox/package/leaderboard/2/` |
| GET | `https://services.facepunch.com/sbox/package/leaderboard/1/{package}/{leaderboard}/u/{steamId}/{mode}` |

## Description
Query leaderboards declared by a package. The v2 endpoint takes a flat query object and is the recommended form. The v1 endpoint is kept for engine backwards-compatibility.

## V2 Query Parameters

| Name           | Type   | Required | Description |
|----------------|--------|----------|-------------|
| ident          | string | Yes      | Full package identifier (`org.package`). Maps to `PackageIdent` server-side. |
| Stat           | string | Yes      | Stat name backing the leaderboard |
| Aggregation    | enum   | No       | `Sum`, `Max`, `Min`, `Last`, `Avg` |
| SortOrder      | enum   | No       | `Desc` (default) or `Asc` |
| DateFilter     | enum   | No       | `None`, `Year`, `Month`, `Week`, `Day` |
| Date           | string | No       | Anchor date for `DateFilter` (ISO 8601) |
| CenterSteamId  | long   | No       | Center the window on this Steam ID |
| Offset         | long   | No       | Pagination offset (default 0) |
| Country        | string | No       | ISO country code filter |
| Count          | int    | No       | Page size (default 50) |
| SteamId        | long   | No       | Querying user's Steam ID |
| Friends        | bool   | No       | If true, restrict to friends of `SteamId` |
| Include        | string | No       | Comma-separated Steam IDs to always include |
| IncludeQuery   | bool   | No       | Debug: return the underlying query string |

## V1 Path Parameters

| Name        | Type   | Required | Description |
|-------------|--------|----------|-------------|
| package     | string | Yes      | Full package identifier |
| leaderboard | string | Yes      | Leaderboard name declared by the package |
| steamId     | long   | Yes      | User to center on |
| mode        | string | Yes      | Mode token (engine-defined; e.g. `Global`, `Friends`) |
| take        | int    | No       | Page size (default 20) |

---

## Example Requests
```
GET /sbox/package/leaderboard/2/?ident=facepunch.testbed&Stat=balls_fired&SortOrder=Desc&Count=20
GET /sbox/package/leaderboard/1/facepunch.testbed/score/u/76561197960279927/Global?take=20
```

---

## Example Response (v2)
```json
{
  "Stat": "balls_fired",
  "TotalEntries": 8109,
  "Entries": [
    {
      "Rank": 1,
      "Value": 12054,
      "SteamId": 76561197960279927,
      "CountryCode": "GB",
      "DisplayName": "garry",
      "Timestamp": "2026-05-22T12:25:58.4+00:00",
      "DataUrl": null
    }
  ],
  "Query": null,
  "DateDescription": "All time"
}
```

## Response Fields (v2 — `LeaderboardResponseEx`)

| Field           | Type   | Description |
|-----------------|--------|-------------|
| Stat            | string | Stat name (echoed) |
| TotalEntries    | long   | Total number of ranked entries |
| Entries         | array  | Leaderboard rows |
| Entries[].Rank  | long   | 1-based rank |
| Entries[].Value | number | Stat value |
| Entries[].SteamId     | long   | Player's Steam ID |
| Entries[].CountryCode | string | ISO country code |
| Entries[].DisplayName | string | Player's display name |
| Entries[].Timestamp   | string | When the value was recorded (ISO 8601) |
| Entries[].DataUrl     | string | Optional pointer to attached run/replay data |
| Query           | string | Echo of the underlying query (when `IncludeQuery=true`) |
| DateDescription | string | Human-readable window summary |

## Response Fields (v1 — `LeaderboardResponseLegacy`)

| Field        | Type   | Description |
|--------------|--------|-------------|
| Title        | string | Leaderboard title |
| DisplayName  | string | Display name |
| Description  | string | Description |
| Unit         | string | Unit of measure |
| TotalEntries | long   | Total ranked entries |
| Entries      | array  | Rows with `{ Me, Rank, Value, ValueString, SteamId, CountryCode, DisplayName }` |

## Error Responses

| Status Code | Description |
|-------------|-------------|
| 404 | Package or stat not found |
| 400 | Invalid query parameters |
| 429 | Rate limit exceeded |

## Notes

- The v2 endpoint binds the path parameter `ident` to the server-side `PackageIdent` property — the URL must use `ident=`.
- An empty leaderboard returns `{ "Stat": "...", "Entries": [] }`.
- Combine `CenterSteamId` and `Count` to fetch a window around a specific player.
- `DateFilter`/`Date` together produce period-scoped boards (e.g. `DateFilter=Week&Date=2026-05-25`).
