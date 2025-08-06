# Package Details

## Endpoint
`GET https://services.facepunch.com/sbox/package/get/{org}.{package}`

## Description
Retrieve detailed information about a specific S&box package including metadata, statistics, version information, screenshots, and dependencies.

## Path Parameters

| Name    | Type   | Required | Description |
|---------|--------|----------|-------------|
| org     | string | Yes      | The organization identifier |
| package | string | Yes      | The package identifier |

---

## Example Request
`GET /sbox/package/get/smallboxstudio.spongebobkrustykrab`

---

## Example Response
```json
{
  "Org": {
    "Ident": "smallboxstudio",
    "Title": "Small Box Studio",
    "Description": "Team Small Box Studio",
    "Thumb": "https://cdn.sbox.game/org/smallboxstudio/logo.7256b703-3987-41c5-8355-397782256ea4.png",
    "Discord": "https://discord.gg/ucvM2sfTBP"
  },
  "Ident": "spongebobkrustykrab",
  "Title": "Sponge Bob Krusty Krab",
  "Summary": "Ohhhh, who lives in a pineapple under the sea ?",
  "Description": "<div><!--block-->Discord : <a href=\"https://discord.gg/UbKbayChqj\">https://discord.gg/UbKbayChqj</a></div>",
  "Thumb": "https://cdn.sbox.game/org/smallboxstudio/spongebobkrustykrab/thumb/1418c92c-de45-4b28-8002-7a52e1974827.png",
  "ThumbWide": "https://cdn.sbox.game/org/smallboxstudio/spongebobkrustykrab/thumb/4079eac1-d49d-4761-aedc-4c27dbdc92b5.png",
  "ThumbTall": "https://cdn.sbox.game/org/smallboxstudio/spongebobkrustykrab/thumb/1418c92c-de45-4b28-8002-7a52e1974827.png",
  "Updated": "2025-07-21T22:47:50.3+00:00",
  "Created": "2025-06-08T14:06:13.7+00:00",
  "UsageStats": {
    "Total": {
      "Users": 1667,
      "Seconds": 701148,
      "Sessions": 1999
    },
    "Month": {
      "Users": 1666,
      "Seconds": 700996
    },
    "Week": {
      "Users": 1666,
      "Seconds": 700996
    },
    "Day": {
      "Users": 1666,
      "Seconds": 700996
    },
    "UsersNow": 7,
    "Trend": 1.93159413337708
  },
  "ReviewStats": {
    "Scores": [
      {
        "Rating": 2,
        "Total": 2
      }
    ]
  },
  "Tags": [
    "bob",
    "hunt",
    "krab",
    "krusty",
    "map",
    "ph",
    "prop",
    "prophunt",
    "props",
    "sponge"
  ],
  "Favourited": 15,
  "VotesUp": 10,
  "Version": {
    "Id": 111746,
    "Changes": "Optimized after the Hammer update and made changes to the restaurant's exterior",
    "FileCount": 227,
    "TotalSize": 175869642,
    "Hash": -503011199095951400,
    "ManifestUrl": "https://cdn.sbox.game/org/smallboxstudio/spongebobkrustykrab/manifest/111746.json",
    "Created": "2025-07-21T22:47:50.3+00:00",
    "EngineVersion": 19,
    "AssetVersionId": 111746
  },
  "Public": true,
  "Screenshots": [
    {
      "Created": "2025-06-17T17:05:06.7Z",
      "Width": 1920,
      "Height": 1080,
      "Url": "https://cdn.sbox.game/org/smallboxstudio/spongebobkrustykrab/screenshots/153a718b-afae-4c31-8271-6324ccc5182a.jpg",
      "Thumb": "https://cdn.sbox.game/org/smallboxstudio/spongebobkrustykrab/screenshots/153a718b-afae-4c31-8271-6324ccc5182a.jpg"
    }
  ],
  "TypeName": "map",
  "PackageReferences": [],
  "EditorReferences": [
    "facepunch.glass_b",
    "facepunch.glass_c",
    "caro.sky_001"
  ]
}
```

## Response Fields

### Basic Information
| Field       | Type   | Description |
|-------------|--------|-------------|
| Ident       | string | Package identifier |
| Title       | string | Package display name |
| Summary     | string | Short description |
| Description | string | Full description (HTML format) |
| TypeName    | string | Package type (e.g., "map", "gamemode", "asset") |
| Public      | bool   | Whether the package is publicly available |
| Created     | string | Package creation timestamp |
| Updated     | string | Last update timestamp |

### Organization Information
| Field       | Type   | Description |
|-------------|--------|-------------|
| Org.Ident   | string | Organization identifier |
| Org.Title   | string | Organization display name |
| Org.Description | string | Organization description |
| Org.Thumb   | string | Organization logo URL |
| Org.Discord | string | Organization Discord invite URL |

### Media
| Field       | Type   | Description |
|-------------|--------|-------------|
| Thumb       | string | Square thumbnail URL |
| ThumbWide   | string | Wide thumbnail URL |
| ThumbTall   | string | Tall thumbnail URL |
| Screenshots | array  | Array of screenshot objects with URL, dimensions, and creation date |

### Statistics
| Field                | Type  | Description |
|---------------------|-------|-------------|
| UsageStats.Total    | object | Total usage statistics (Users, Seconds, Sessions) |
| UsageStats.Month    | object | Monthly usage statistics |
| UsageStats.Week     | object | Weekly usage statistics |
| UsageStats.Day      | object | Daily usage statistics |
| UsageStats.Trend    | float | Usage trend indicator |
| Favourited          | int   | Number of users who favorited this package |
| VotesUp             | int   | Number of upvotes |
| ReviewStats         | object | Review scores and totals |
| UsageStats.UsersNow | int   | Current active users |

### Version Information
| Field              | Type   | Description |
|-------------------|--------|-------------|
| Version.Id        | int    | Version identifier |
| Version.Changes   | string | Changelog for this version |
| Version.FileCount | int    | Number of files in the package |
| Version.TotalSize | int    | Total package size in bytes |
| Version.Hash      | long   | Package hash |
| Version.ManifestUrl | string | URL to package manifest |
| Version.EngineVersion | int | S&box engine version requirement |

### Dependencies
| Field             | Type  | Description |
|-------------------|-------|-------------|
| Tags              | array | Package tags for searchability |
| PackageReferences | array | Runtime package dependencies |
| EditorReferences  | array | Editor-time package dependencies |

## Error Responses

| Status Code | Description |
|-------------|-------------|
| 404         | Package not found |
| 400         | Invalid package identifier format |
| 429         | Rate limit exceeded |

## Notes

- Package identifiers follow the format `{organization}.{package}`
- The `Description` field contains HTML content
- `UsageStats` provides comprehensive analytics about package usage
- `EditorReferences` are dependencies used during development but not at runtime
- Screenshot URLs point to full-resolution images; `Thumb` provides smaller versions
- File sizes are in