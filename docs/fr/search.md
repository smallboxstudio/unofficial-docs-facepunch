# Recherche de Packages

## Endpoint
`GET https://services.facepunch.com/sbox/package/find`

## Description
Rechercher des packages (cartes, modes de jeu, assets) disponibles sur la plateforme S&box, avec filtrage par type, tri, pagination, etc.

## Paramètres de Requête

| Nom   | Type   | Requis | Description |
|-------|--------|--------|-------------|
| q     | string | Oui    | Requête de recherche. Peut inclure `type:<type>` et `sort:<ordre>`. |
| take  | int    | Oui    | Nombre maximum de résultats à retourner. |
| skip  | int    | Oui    | Décalage pour la pagination. |

---

## Exemple de Requête
`GET /sbox/package/find?q=type:map%20sort:trending&take=6&skip=0`

---

## Exemple de Réponse
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
      "TypeName": "map",
      "Updated": "2025-07-21T22:47:55.2+00:00",
      "Created": "2025-06-08T14:06:13.7+00:00",
      "Tags": [
        "bob",
        "hunt",
        "krab",
        "krusty",
        "map"
      ],
      "Favourited": 15,
      "VotesUp": 10,
      "Public": true
    }
  ]
}
```

## Notes

- Le paramètre `q` peut être utilisé pour filtrer par type de package (ex: `type:map`, `type:gamemode`) et ordre de tri (ex: `sort:trending`, `sort:newest`).
- Le paramètre `take` spécifie combien de résultats retourner, tandis que `skip` est utilisé pour la pagination.
- La réponse inclut les détails du package comme l'organisation, titre, résumé, miniature, type, et diverses statistiques comme les votes et favoris.