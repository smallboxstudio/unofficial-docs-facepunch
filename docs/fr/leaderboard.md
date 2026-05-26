# Classements

## Endpoints

| Méthode | Chemin |
|---------|--------|
| GET | `https://services.facepunch.com/sbox/package/leaderboard/2/` |
| GET | `https://services.facepunch.com/sbox/package/leaderboard/1/{package}/{leaderboard}/u/{steamId}/{mode}` |

## Description
Interroger les classements déclarés par un package. La version 2 accepte un objet de requête plat (forme recommandée). La version 1 est conservée pour compatibilité avec le moteur.

## Paramètres de Requête (v2)

| Nom            | Type   | Requis | Description |
|----------------|--------|--------|-------------|
| ident          | string | Oui    | Identifiant complet (`org.package`). Mappé à `PackageIdent` côté serveur. |
| Stat           | string | Oui    | Nom de la statistique associée au classement |
| Aggregation    | enum   | Non    | `Sum`, `Max`, `Min`, `Last`, `Avg` |
| SortOrder      | enum   | Non    | `Desc` (défaut) ou `Asc` |
| DateFilter     | enum   | Non    | `None`, `Year`, `Month`, `Week`, `Day` |
| Date           | string | Non    | Date d'ancrage pour `DateFilter` (ISO 8601) |
| CenterSteamId  | long   | Non    | Centrer la fenêtre sur ce Steam ID |
| Offset         | long   | Non    | Décalage de pagination (défaut 0) |
| Country        | string | Non    | Code pays ISO (filtre) |
| Count          | int    | Non    | Taille de page (défaut 50) |
| SteamId        | long   | Non    | Steam ID de l'utilisateur qui requête |
| Friends        | bool   | Non    | Si vrai, restreint aux amis de `SteamId` |
| Include        | string | Non    | Liste de Steam IDs (séparés par virgule) toujours inclus |
| IncludeQuery   | bool   | Non    | Debug : renvoie la requête sous-jacente |

## Paramètres de Chemin (v1)

| Nom         | Type   | Requis | Description |
|-------------|--------|--------|-------------|
| package     | string | Oui    | Identifiant complet du package |
| leaderboard | string | Oui    | Nom du classement déclaré par le package |
| steamId     | long   | Oui    | Steam ID à centrer |
| mode        | string | Oui    | Mode (défini par le moteur ; ex. `Global`, `Friends`) |
| take        | int    | Non    | Taille de page (défaut 20) |

---

## Exemples de Requête
```
GET /sbox/package/leaderboard/2/?ident=facepunch.testbed&Stat=balls_fired&SortOrder=Desc&Count=20
GET /sbox/package/leaderboard/1/facepunch.testbed/score/u/76561197960279927/Global?take=20
```

---

## Exemple de Réponse (v2)
```json
{
  "Stat": "balls_fired",
  "TotalEntries": 8109,
  "Entries": [
    {
      "Rank": 1,
      "Value": 12054,
      "SteamId": 76561197960279927,
      "CountryCode": "GB",
      "DisplayName": "garry",
      "Timestamp": "2026-05-22T12:25:58.4+00:00",
      "DataUrl": null
    }
  ],
  "Query": null,
  "DateDescription": "All time"
}
```

## Champs de Réponse (v2 — `LeaderboardResponseEx`)

| Champ           | Type   | Description |
|-----------------|--------|-------------|
| Stat            | string | Nom de la stat (rappelé) |
| TotalEntries    | long   | Nombre total d'entrées classées |
| Entries         | array  | Lignes du classement |
| Entries[].Rank  | long   | Rang (base 1) |
| Entries[].Value | number | Valeur de la stat |
| Entries[].SteamId     | long   | Steam ID du joueur |
| Entries[].CountryCode | string | Code pays ISO |
| Entries[].DisplayName | string | Nom d'affichage |
| Entries[].Timestamp   | string | Date d'enregistrement (ISO 8601) |
| Entries[].DataUrl     | string | URL optionnelle vers des données associées (run/replay) |
| Query           | string | Requête sous-jacente (si `IncludeQuery=true`) |
| DateDescription | string | Résumé textuel de la fenêtre |

## Champs de Réponse (v1 — `LeaderboardResponseLegacy`)

| Champ        | Type   | Description |
|--------------|--------|-------------|
| Title        | string | Titre du classement |
| DisplayName  | string | Nom d'affichage |
| Description  | string | Description |
| Unit         | string | Unité |
| TotalEntries | long   | Nombre total d'entrées |
| Entries      | array  | Lignes `{ Me, Rank, Value, ValueString, SteamId, CountryCode, DisplayName }` |

## Réponses d'Erreur

| Code Statut | Description |
|-------------|-------------|
| 404 | Package ou statistique introuvable |
| 400 | Paramètres de requête invalides |
| 429 | Limite de taux dépassée |

## Notes

- La v2 mappe le paramètre `ident` à la propriété `PackageIdent` côté serveur — utilisez bien `ident=` dans l'URL.
- Un classement vide renvoie `{ "Stat": "...", "Entries": [] }`.
- Combinez `CenterSteamId` et `Count` pour obtenir une fenêtre autour d'un joueur précis.
- `DateFilter`/`Date` ensemble produisent un classement périodique (ex. `DateFilter=Week&Date=2026-05-25`).
