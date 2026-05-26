# Versions de Package

## Endpoint
`GET https://services.facepunch.com/sbox/package/versions/2/{packageIdent}`

## Description
Récupérer l'historique des versions publiées d'un package S&box : nombre de fichiers, taille totale, URL du manifeste, version du moteur et métadonnées propres à chaque version.

## Paramètres de Chemin

| Nom          | Type   | Requis | Description |
|--------------|--------|--------|-------------|
| packageIdent | string | Oui    | Identifiant complet du package (`org.package`) |

---

## Exemple de Requête
`GET /sbox/package/versions/2/facepunch.testbed`

---

## Exemple de Réponse
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

## Champs de Réponse

| Champ          | Type   | Description |
|----------------|--------|-------------|
| Id             | long   | Identifiant de la version |
| Changes        | string | Notes de changement (texte libre) |
| FileCount      | long   | Nombre de fichiers dans cette version |
| TotalSize      | long   | Taille totale en octets |
| Hash           | long   | Hash du contenu (entier signé 64-bit) |
| ManifestUrl    | string | URL absolue du manifeste JSON sur le CDN S&box |
| Created        | string | Date de publication (ISO 8601) |
| EngineVersion  | int    | Version requise du moteur |
| Meta           | string | Métadonnées encodées en JSON (type de jeu, nombre de joueurs, liste de maps, etc.) |
| AssetVersionId | long   | Alias historique de `Id` pour compatibilité ascendante |

## Réponses d'Erreur

| Code Statut | Description |
|-------------|-------------|
| 404 | Package introuvable |
| 400 | Identifiant invalide |
| 429 | Limite de taux dépassée |

## Notes

- Les entrées sont triées de la plus récente à la plus ancienne.
- `Meta` est une *chaîne* JSON, pas un objet : il faut la parser côté client. Sa forme varie selon le type de package (les jeux incluent typiquement `MaxPlayers`, `MapList`, `RankType`, …).
- `ManifestUrl` pointe vers le manifeste d'assets que le moteur consulte pour récupérer le contenu du package.
