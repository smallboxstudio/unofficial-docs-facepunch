# Organization News

## Endpoint
`GET https://services.facepunch.com/sbox/news/organization/{orgIdent}`

## Description
Retrieve news articles and updates published by a specific organization, including announcements, package updates, and other content from the organization.

## Path Parameters

| Name     | Type   | Required | Description |
|----------|--------|----------|-------------|
| orgIdent | string | Yes      | The organization identifier |

---

## Example Request
`GET /sbox/news/organization/smallboxstudio`

---

## Example Response
```json
[
  {
    "Id": "10c7ac25-2c71-4256-b7c9-1de99407ca33",
    "Created": "2025-07-09T09:11:24.6+00:00",
    "Title": "Update 1.0",
    "Summary": "",
    "Url": "/smallboxstudio/rpunioncity/news/update-10-e02a59c0",
    "Author": {
      "Id": 76561198050191277,
      "Name": "TwinsD",
      "Url": "/u/SecretDefense",
      "Avatar": "https://avatars.steamstatic.com/d3bb2c897bc789bcc3d8e2a459820d9f01798f9a_medium.jpg",
      "Score": 4549
    },
    "Package": "smallboxstudio.rpunioncity",
    "Media": "https://cdn.sbox.game/upload/i/a6be19b2/630f/485d/b957/d9045d282820",
    "Sections": [
      {
        "Id": "9a728432-20c6-419b-8fe6-2a1b02d6e0d3",
        "Title": "update 1.0",
        "Author": {
          "Id": 76561198050191277,
          "Name": "TwinsD",
          "Url": "/u/SecretDefense",
          "Avatar": "https://avatars.steamstatic.com/d3bb2c897bc789bcc3d8e2a459820d9f01798f9a_medium.jpg",
          "Score": 4549
        },
        "SortOrder": 5000,
        "Contents": "<div><!--block-->– Add lights.<figure data-trix-attachment=\"...\">...</figure>– Remove unnecessary props.<br><br>– Remove unnecessary decals.<br><br>– Add physics props.<figure data-trix-attachment=\"...\">...</figure>– Optimized the map following the Hammer update.</div>",
        "Slug": "update-10"
      }
    ]
  }
]
```

## Response Fields

### Article Information
| Field   | Type   | Description |
|---------|--------|-------------|
| Id      | string | Unique article identifier (UUID) |
| Created | string | Article creation timestamp |
| Title   | string | Article title |
| Summary | string | Article summary/excerpt |
| Url     | string | Relative URL to the full article |
| Package | string | Associated package identifier (format: org.package) |
| Media   | string | Featured media URL for the article |

### Author Information
| Field        | Type   | Description |
|--------------|--------|-------------|
| Author.Id    | long   | Author's Steam ID |
| Author.Name  | string | Author's display name |
| Author.Url   | string | Relative URL to author's profile |
| Author.Avatar| string | Author's avatar image URL |
| Author.Score | int    | Author's community score |

### Article Sections
| Field              | Type   | Description |
|--------------------|--------|-------------|
| Sections[].Id      | string | Section unique identifier |
| Sections[].Title   | string | Section title |
| Sections[].Author  | object | Section author (same structure as main author) |
| Sections[].SortOrder| int   | Display order of the section |
| Sections[].Contents| string | Section content (HTML format) |
| Sections[].Slug    | string | URL-friendly section identifier |

## Content Format

The `Contents` field contains rich HTML content that may include:
- Text formatting (bold, italic, etc.)
- Images with metadata (`data-trix-attachment`)
- Videos and other media files
- Links and references
- File attachments with size information

## Error Responses

| Status Code | Description |
|-------------|-------------|
| 404         | Organization not found |
| 400         | Invalid organization identifier |
| 429         | Rate limit exceeded |

## Notes

- The response is an array of news articles, ordered by creation date (newest first)
- Articles may be associated with specific packages through the `Package` field
- The `Contents` field uses Trix editor HTML format with embedded media metadata
- Media URLs point to S&box's CDN for images, videos, and other attachments
- Some articles may have empty `Summary` fields
- `SortOrder` determines the display sequence of sections within an article