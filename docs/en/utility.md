# Utility

## Endpoint
`GET https://services.facepunch.com/sbox/utility/avatars`

## Description
Return a list of randomized Citizen avatar configurations, suitable for seeding NPCs or generating example characters in tooling. Each entry is a JSON-encoded string containing the same shape as the `Avatar` field of `/sbox/player/{id}/overview`.

## Request

No path or query parameters.

---

## Example Request
`GET /sbox/utility/avatars`

---

## Example Response
```json
[
  "{\"Items\":[{\"p\":\"models/citizen_clothes/body/human03.clothing\"},{\"p\":\"models/citizen_clothes/hat/gasmask/gasmask.clothing\"}],\"Height\":0.58,\"DisplayName\":\"Citizen\",\"Age\":0.41}"
]
```

## Response

Returns `string[]`. Each string is JSON that, when parsed, has roughly this shape:

| Field        | Type   | Description |
|--------------|--------|-------------|
| Items        | array  | Clothing items: `{ p: "models/.../<name>.clothing", t?: <tint> }` |
| Height       | float  | Height multiplier (0–1) |
| DisplayName  | string | Random display name |
| Age          | float  | Age multiplier (0–1) |
| Tint         | float  | Skin tint (0–1) |
| PrefersHuman | bool   | Whether the model prefers the human Citizen base |

## Error Responses

| Status Code | Description |
|-------------|-------------|
| 429 | Rate limit exceeded |
| 500 | Internal server error |

## Notes

- The response is an array of strings, **not** an array of objects — each element must be `JSON.parse`d separately.
- This is the same encoding used by the `Avatar` string in player overview responses.
- The list size is fixed server-side; do not assume a specific count.
