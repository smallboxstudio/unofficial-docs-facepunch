# Avis de Package

## Endpoints

| Méthode | Chemin |
|---------|--------|
| GET | `https://services.facepunch.com/sbox/package/reviews/{packageIdent}` |
| GET | `https://services.facepunch.com/sbox/package/reviews/{packageIdent}/{steamId}` |

## Description
Récupérer les avis utilisateurs publiés pour un package S&box. Le premier endpoint retourne une liste paginée filtrable par score et tags. Le second retourne l'avis d'un seul utilisateur sur ce package.

## Paramètres de Chemin

| Nom           | Type   | Requis | Description |
|---------------|--------|--------|-------------|
| packageIdent  | string | Oui    | Identifiant complet du package (`org.package`) |
| steamId       | long   | Oui (avis unique) | Steam ID (64-bit) de l'auteur de l'avis |

## Paramètres de Requête (liste)

| Nom       | Type | Requis | Défaut | Description |
|-----------|------|--------|--------|-------------|
| skip      | int  | Oui    | —      | Décalage pour la pagination |
| take      | int  | Oui    | —      | Nombre maximum d'avis à retourner |
| score     | int  | Non    | 0      | Filtrer par `ReviewScore` (0 = tous, 1 = Négatif, 2 = Positif, 3 = Promesse) |
| positives | int  | Non    | 0      | Masque de bits `ReviewPositiveTags` |
| negatives | int  | Non    | 0      | Masque de bits `ReviewNegativeTags` |

---

## Exemples de Requête
```
GET /sbox/package/reviews/facepunch.testbed/?skip=0&take=10
GET /sbox/package/reviews/facepunch.sandbox/76561197960279927
```

---

## Exemple de Réponse (liste)
```json
{
  "Count": 139,
  "Skip": 0,
  "Take": 10,
  "Entries": [
    {
      "Player": {
        "Id": 76561199134354514,
        "Name": "Strange™",
        "Url": "/u/76561199134354514",
        "Avatar": "https://avatars.steamstatic.com/b7066fb6ff81e0241bea7620e0bda9e06b669c13_medium.jpg",
        "Online": true,
        "Score": 1455
      },
      "SteamId": 76561199134354514,
      "PackageId": 20223,
      "Content": "Useful for helping me understand physics of certain objects:)",
      "Score": 2,
      "SecondsPlayed": 1784,
      "Created": "2026-05-25T18:18:51.13+00:00",
      "Updated": "2026-05-25T18:18:51.13+00:00",
      "Positives": 4095
    }
  ]
}
```

## Exemple de Réponse (avis unique)
```json
{
  "Player": {
    "Id": 76561197960279927,
    "Name": "garry",
    "Url": "/u/garry",
    "Avatar": "https://avatars.steamstatic.com/adfd2545a3a5f0eb07ad042873be6aa4e03ef660_medium.jpg",
    "Online": true,
    "Score": 1375
  },
  "SteamId": 76561197960279927,
  "PackageId": 12,
  "Content": "wwow!",
  "Score": 3,
  "SecondsPlayed": 51871,
  "Created": "2026-04-30T11:06:03.8+01:00",
  "Updated": "2026-05-01T09:52:16.7+00:00",
  "Positives": 2354,
  "Negatives": 2178
}
```

## Champs de Réponse

### Enveloppe de liste
| Champ   | Type  | Description |
|---------|-------|-------------|
| Count   | int   | Nombre total d'avis pour le package |
| Skip    | int   | Décalage de pagination (rappelé) |
| Take    | int   | Taille de page (rappelée) |
| Entries | array | Tableau de `PackageReviewDto` |

### Avis (`PackageReviewDto`)
| Champ         | Type   | Description |
|---------------|--------|-------------|
| Player        | object | Profil de l'auteur (voir endpoint Joueurs) |
| SteamId       | long   | Steam ID de l'auteur |
| PackageId     | long   | ID numérique interne du package |
| Content       | string | Texte de l'avis (peut être vide) |
| Score         | int    | Valeur `ReviewScore` : 1 = Négatif, 2 = Positif, 3 = Promesse |
| SecondsPlayed | int    | Temps de jeu avant publication de l'avis |
| Created       | string | Date de création (ISO 8601) |
| Updated       | string | Date de mise à jour (ISO 8601) |
| Positives     | int    | Masque de bits `ReviewPositiveTags` |
| Negatives     | int    | Masque de bits `ReviewNegativeTags` |

### `ReviewPositiveTags` (drapeaux binaires)
| Bit | Valeur | Nom | Icône |
|-----|--------|-----|-------|
| 0  | 1     | Graphics      | palette |
| 1  | 2     | Audio         | volume_up |
| 2  | 4     | Gameplay      | sports_esports |
| 3  | 8     | Story         | menu_book |
| 4  | 16    | Multiplayer   | groups |
| 5  | 32    | Originality   | lightbulb |
| 6  | 64    | Performance   | speed |
| 7  | 128   | Polish        | auto_awesome |
| 8  | 256   | Addictive     | favorite |
| 9  | 512   | Replayability | replay |
| 10 | 1024  | Controls      | gamepad |
| 11 | 2048  | Updates       | update |

### `ReviewNegativeTags` (drapeaux binaires)
| Bit | Valeur | Nom | Icône |
|-----|--------|-----|-------|
| 1  | 2     | Unfinished    | construction |
| 2  | 4     | Unoptimized   | slow_motion_video |
| 3  | 8     | Bad Controls  | gamepad |
| 4  | 16    | Confusing     | help |
| 5  | 32    | Slop          | mop |
| 6  | 64    | Generated Art | smart_toy |
| 7  | 128   | Pay to Win    | paid |
| 8  | 256   | Stolen        | report |
| 9  | 512   | Errors        | error |
| 10 | 1024  | Load Times    | hourglass_top |
| 11 | 2048  | Buggy         | bug_report |
| 12 | 4096  | Clicker       | touch_app |
| 13 | 8192  | Idle          | autorenew |

## Réponses d'Erreur

| Code Statut | Description |
|-------------|-------------|
| 404 | Package ou avis introuvable |
| 400 | Identifiant ou paramètres de requête invalides |
| 429 | Limite de taux dépassée |

## Notes

- `Positives` et `Negatives` sont des `[Flags]` : combinez les bits pour filtrer plusieurs tags.
- L'endpoint d'avis unique retourne `404` si l'utilisateur n'a pas évalué le package.
- Les avis marqués `DisplayMode = HiddenFromPublic` sont exclus des réponses publiques.
- `Score` provient de l'énum `ReviewScore` du moteur : `Negative = 1`, `Positive = 2`, `Promise = 3`.
