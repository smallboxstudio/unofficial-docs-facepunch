# Détails du Package

## Endpoint
`GET https://services.facepunch.com/sbox/package/get/{org}.{package}`

## Description
Récupérer des informations détaillées sur un package S&box spécifique incluant les métadonnées, statistiques, informations de version, captures d'écran et dépendances.

## Paramètres de Chemin

| Nom     | Type   | Requis | Description |
|---------|--------|--------|-------------|
| org     | string | Oui    | L'identifiant de l'organisation |
| package | string | Oui    | L'identifiant du package |

---

## Exemple de Requête
`GET /sbox/package/get/smallboxstudio.spongebobkrustykrab`

---

## Exemple de Réponse
```json
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

## Champs de Réponse

### Informations de Base
| Champ       | Type   | Description |
|-------------|--------|-------------|
| Ident       | string | Identifiant du package |
| Title       | string | Nom d'affichage du package |
| Summary     | string | Description courte |
| TypeName    | string | Type de package (ex: "map", "gamemode", "asset") |
| Public      | bool   | Si le package est disponible publiquement |
| Created     | string | Horodatage de création du package |
| Updated     | string | Horodatage de dernière mise à jour |

### Informations Organisation
| Champ           | Type   | Description |
|-----------------|--------|-------------|
| Org.Ident       | string | Identifiant de l'organisation |
| Org.Title       | string | Nom d'affichage de l'organisation |
| Org.Description | string | Description de l'organisation |

### Statistiques
| Champ                | Type   | Description |
|---------------------|--------|-------------|
| UsageStats.Total    | object | Statistiques d'usage totales |
| Favourited          | int    | Nombre d'utilisateurs ayant mis en favori |
| VotesUp             | int    | Nombre de votes positifs |
| ReviewStats         | object | Statistiques de revue et scores |
| UsageStats.UsersNow | int   | Utilisateurs actifs actuels |

## Notes

- Les identifiants de package suivent le format `{organisation}.{package}`
- Les `UsageStats` fournissent des analyses complètes sur l'usage du package
- Les tailles de fichiers sont en octets