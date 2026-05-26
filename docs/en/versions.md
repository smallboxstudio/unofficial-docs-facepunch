# Package Versions

## Endpoint
`GET https://services.facepunch.com/sbox/package/versions/2/{packageIdent}`

## Description
Retrieve the published version history of a S&box package — file count, total size, manifest URL, engine version, and arbitrary version metadata for each entry.

## Path Parameters

| Name         | Type   | Required | Description |
|--------------|--------|----------|-------------|
| packageIdent | string | Yes      | Full package identifier (`org.package`) |

---

## Example Request
`GET /sbox/package/versions/2/facepunch.testbed`

---

## Example Response
```json
[
  {
    "Id": 205910,
    "Changes": "Changes on 2026-04-26",
    "FileCount": 3244,
    "TotalSize": 3202714919,
    "Hash": 8981653667619779619,
    "ManifestUrl": "https://cdn.sbox.game/org/facepunch/testbed/manifest/205910.json",
    "Created": "2026-04-26T07:21:22.1+00:00",
    "EngineVersion": 25,
    "Meta": "{\"MaxPlayers\":8,\"MinPlayers\":1,\"GameNetworkType\":\"Multiplayer\"}",
    "AssetVersionId": 205910
  }
]
```

## Response Fields

| Field          | Type   | Description |
|----------------|--------|-------------|
| Id             | long   | Version identifier |
| Changes        | string | Free-form changelog text |
| FileCount      | long   | Number of files in this version |
| TotalSize      | long   | Total size in bytes |
| Hash           | long   | Content hash (signed 64-bit) |
| ManifestUrl    | string | Absolute URL to the version manifest JSON on the S&box CDN |
| Created        | string | Publication timestamp (ISO 8601) |
| EngineVersion  | int    | Required engine version |
| Meta           | string | JSON-encoded metadata blob (game type, player counts, map list, etc.) |
| AssetVersionId | long   | Legacy alias of `Id` for backwards compatibility |

## Error Responses

| Status Code | Description |
|-------------|-------------|
| 404 | Package not found |
| 400 | Invalid package identifier |
| 429 | Rate limit exceeded |

## Notes

- Entries are ordered newest first.
- `Meta` is a JSON string, not an object — parse it client-side. Its shape varies by package type (games often include `MaxPlayers`, `MapList`, `RankType`, etc.).
- `ManifestUrl` points to the per-version asset manifest used by the engine to fetch package contents.
