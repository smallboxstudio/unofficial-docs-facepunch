# Package Search

## Endpoint
`GET https://services.facepunch.com/sbox/package/find`


## Description
Search for packages (maps, gamemodes, assets) available on the S&box platform, with filtering by type, sorting, pagination, etc.

## Query Parameters

| Name  | Type   | Required | Description |
|-------|--------|----------|-------------|
| q     | string | Yes      | Search query. Can include `type:<type>` and `sort:<order>`. |
| take  | int    | Yes      | Maximum number of results to return. |
| skip  | int    | Yes      | Offset for pagination. |

---

## Example Request
`GET /sbox/package/find?q=type:map%20sort:trending&take=6&skip=0`


---

## Example Response
```json
{
  "Packages": [
    {
      "Org": {
        "Ident": "smallboxstudio",
        "Title": "Small Box Studio",
        "Description": "Team Small Box Studio",
        "Thumb": "https://cdn.sbox.game/org/smallboxstudio/logo.7256b703-3987-41c5-8355-397782256ea4.png",
        "Discord": "https://discord.gg/ucvM2sfTBP"
      },
      "Ident": "spongebobkrustykrab",
      "FullIdent": "smallboxstudio.spongebobkrustykrab",
      "Title": "Sponge Bob Krusty Krab",
      "Summary": "Ohhhh, who lives in a pineapple under the sea ?",
      "Thumb": "https://cdn.sbox.game/org/smallboxstudio/spongebobkrustykrab/thumb/1418c92c-de45-4b28-8002-7a52e1974827.png",
      "ThumbWide": "https://cdn.sbox.game/org/smallboxstudio/spongebobkrustykrab/thumb/4079eac1-d49d-4761-aedc-4c27dbdc92b5.png",
      "ThumbTall": "https://cdn.sbox.game/org/smallboxstudio/spongebobkrustykrab/thumb/1418c92c-de45-4b28-8002-7a52e1974827.png",
      "TypeName": "map",
      "Updated": "2025-07-21T22:47:55.2+00:00",
      "Created": "2025-06-08T14:06:13.7+00:00",
      "UsageStats": {

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
      "Referencing": 12,
      "VotesUp": 10,
      "Public": true
    },
```


## Notes

- The `q` parameter can be used to filter by package type (e.g., `type:map`, `type:gamemode`) and sort order (e.g., `sort:trending`, `sort:newest`).
- The `take` parameter specifies how many results to return, while `skip` is used for pagination.
- The response includes package details such as organization, title, summary, thumbnail, type, and various statistics like votes and favorites.
- The `TotalCount` field indicates the total number of packages matching the search criteria.


Do you want me to also add a **"Filter Options"** table listing all possible `type:` and `sort:` values so this page is more complete? That would make the documentation much more useful.
