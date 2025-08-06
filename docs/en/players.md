# Player Information

## Endpoint
`GET https://services.facepunch.com/sbox/player/{steamId}`

## Description
Retrieve detailed information about a specific S&box player using their Steam ID, including profile data and score.

## Path Parameters

| Name    | Type   | Required | Description |
|---------|--------|----------|-------------|
| steamId | string | Yes      | The player's Steam ID (64-bit format). |

---

## Example Request
`GET /sbox/player/76561197960279927`

---

## Example Response
```json
{
  "Id": 76561197960279927,
  "Name": "garry",
  "Url": "/u/garry",
  "Avatar": "https://avatars.steamstatic.com/adfd2545a3a5f0eb07ad042873be6aa4e03ef660_medium.jpg",
  "Score": 1200
}
```

## Response Fields

| Field  | Type   | Description |
|--------|--------|-------------|
| Id     | long   | The player's Steam ID (64-bit format) |
| Name   | string | Player's display name |
| Url    | string | Relative URL to the player's profile page |
| Avatar | string | URL to the player's Steam avatar image |
| Score  | int    | Player's current score/reputation |

## Error Responses

| Status Code | Description |
|-------------|-------------|
| 404         | Player not found or invalid Steam ID |
| 400         | Invalid Steam ID format |
| 429         | Rate limit exceeded |

## Notes

- Steam IDs must be in 64-bit format (17-digit number starting with 765...)
- The `Url` field provides a relative path that can be appended to the base S&box website URL
- Avatar images are sourced from Steam's CDN
- Score represents the player's reputation or activity level within the