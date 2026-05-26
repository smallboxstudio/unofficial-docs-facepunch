# Package Search

## Endpoint
`GET https://services.facepunch.com/sbox/package/find/2`

> The legacy `/sbox/package/find` endpoint still resolves to the same handler.

## Description
Search for packages (maps, gamemodes, games, assets…) on the S&box platform with filtering, sorting, and pagination. The query is expressed as a single text string with key prefixes (`type:`, `sort:`, `+tag`, `org:`, …).

## Query Parameters

| Name | Type   | Required | Default | Description |
|------|--------|----------|---------|-------------|
| q    | string | Yes      | —       | Search query. Supports key:value tokens (see below). |
| take | int    | No       | 100     | Maximum number of results to return. |
| skip | int    | No       | 0       | Offset for pagination. |

### Supported `q` tokens

| Token form    | Effect |
|---------------|--------|
| `<text>`      | Free text search across title, summary and description |
| `+<tag>`      | Require tag (multiple `+` tokens compose as AND) |
| `type:<type>` | Filter by package type (`game`, `map`, `gamemode`, `tool`, `tutorial`, `clothing`, …) |
| `sort:<order>` | Sort mode (see table below) |
| `org:<ident>` | Restrict to a specific organization |
| `asset:<name>` | Filter by primary asset type |
| `contest:<name>` | Restrict to a contest |
| `in:<collection>` | Restrict to a collection |
| `target:<pkg>` | Restrict to content built for a specific target package |
| `+game:<pkg>` | Restrict to content created for this game (`any` to clear) |
| `is:unplayed` | Hide packages already played by the requesting user (auth) |
| `is:fave` | Restrict to the requesting user's favourites (auth) |

### `sort:` values

| Value | Meaning |
|-------|---------|
| `trending` | Trending (default popular variant) |
| `popular` | Popular |
| `newest` / `oldest` | Sort by creation date |
| `updated` | Recently updated |
| `friends` | Friend-played packages (auth) |
| `random` | Randomized |
| `upvotes` / `downvotes` | Vote counts |
| `favcount` | Favourite count |
| `live`, `referenced`, `referencing`, `user`, `used`, `played` | Recently used |
| `rankd` / `rankday`, `rankw` / `rankweek`, `rankm` / `rankmonth` | Period rank |
| `spawns`, `spawnsday`, `spawnsweek`, `spawnsmonth` | Spawn counts |
| `playersnow` | Live concurrent players |
| `bestrated` / `rated` | Wilson lower bound on review proportion |
| `mostreviewed` / `reviewed` | Total review count |
| `quality` | Composite quality score |
| `hiddengem` / `underrated` | Well-reviewed but low-traffic |

---

## Example Request
`GET /sbox/package/find/2?q=type:map+sort:trending&take=6&skip=0`

---

## Example Response (truncated)
```json
{
  "Packages": [
    {
      "Org": {
        "Ident": "smallboxstudio",
        "Title": "Small Box Studio",
        "Thumb": "https://cdn.sbox.game/org/smallboxstudio/logo.png",
        "Discord": "https://discord.gg/ucvM2sfTBP"
      },
      "Ident": "spongebobkrustykrab",
      "FullIdent": "smallboxstudio.spongebobkrustykrab",
      "Title": "Sponge Bob Krusty Krab",
      "Summary": "Ohhhh, who lives in a pineapple under the sea ?",
      "Thumb": "https://cdn.sbox.game/.../thumb/...png",
      "TypeName": "map",
      "Updated": "2025-07-21T22:47:55.2+00:00",
      "Created": "2025-06-08T14:06:13.7+00:00",
      "UsageStats": {},
      "Tags": ["map", "ph", "prop", "prophunt"],
      "Favourited": 15,
      "Referencing": 12,
      "VotesUp": 10,
      "Public": true
    }
  ],
  "TotalCount": 1347,
  "Facets": [
    {
      "Name": "category",
      "Title": "Category",
      "Entries": [
        { "Name": "wall", "Title": "Wall", "Icon": "...", "Count": 432, "Children": [] }
      ]
    }
  ],
  "Tags": { "physics": 1820, "multiplayer": 1240 },
  "Orders": [],
  "Properties": []
}
```

## Response Fields

### Root (`PackageFindResult`)
| Field      | Type    | Description |
|------------|---------|-------------|
| Packages   | array   | List of `PackageWrapMinimal` entries (see Package endpoint) |
| TotalCount | long    | Total number of packages matching the query |
| Facets     | array   | Faceted filter groups for refining the search |
| Tags       | object  | Map of `tagName → count` for tag suggestions |
| Orders     | array   | Available sort orders for the active query |
| Properties | array   | Property tags surfaced by the active query |

### Facet (`PackageFacet`)
| Field   | Type   | Description |
|---------|--------|-------------|
| Name    | string | Facet identifier |
| Title   | string | Display title |
| Entries | array  | Pre-sorted entries — do not reorder client-side |

### Facet entry
| Field    | Type   | Description |
|----------|--------|-------------|
| Name     | string | Token to apply when filtering (e.g. `category:wall`) |
| Title    | string | Display title |
| Icon     | string | Optional icon |
| Count    | int    | Number of packages in this bucket |
| Children | array  | Nested entries (same shape, recursive) |

## Notes

- Tokens in `q` are separated by spaces. URL-encode spaces as `%20` or `+`.
- Unknown `key:value` tokens are converted into facet filters automatically.
- The `Facets`/`Tags`/`Orders`/`Properties` fields may be empty if the query does not request them or the result set is too narrow.
- For full per-package details, follow up with `GET /sbox/package/get/2/{org.package}`.
