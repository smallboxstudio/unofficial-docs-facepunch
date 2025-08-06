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
{
  "Org": {
    "Ident": "smallboxstudio",
    "Title": "Small Box Studio",
    "Description": "Team Small Box Studio"
  },
  "Ident": "spongebobkrustykrab",
  "Title": "Sponge Bob Krusty Krab",
  "Summary": "Ohhhh, who lives in a pineapple under the sea ?",
  "TypeName": "map",
  "Updated": "2025-07-21T22:47:50.3+00:00",
  "Created": "2025-06-08T14:06:13.7+00:00",
  "UsageStats": {
    "Total": {
      "Users": 1667,
      "Seconds": 701148,
      "Sessions": 1999
    }
  },
  "Favourited": 15,
  "VotesUp": 10,
  "Public": true
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

## Notes

- Les identifiants de package suivent le format `{organisation}.{package}`
- Les `UsageStats` fournissent des analyses complètes sur l'usage du package
- Les tailles de fichiers sont en octets