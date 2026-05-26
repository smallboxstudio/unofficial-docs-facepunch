# Informations Joueur

## Endpoints

| Méthode | Chemin |
|---------|--------|
| GET | `https://services.facepunch.com/sbox/player/{steamId}` |
| GET | `https://services.facepunch.com/sbox/player/{steamId}/overview` |
| GET | `https://services.facepunch.com/sbox/player/{steamId}/achievementprogress` |

## Description
Récupérer les informations d'un joueur S&box par Steam ID : profil de base, agrégats globaux, progression des succès par package.

## Paramètres de Chemin

| Nom     | Type | Requis | Description |
|---------|------|--------|-------------|
| steamId | long | Oui    | Steam ID du joueur (format 64-bit à 17 chiffres) |

## Paramètres de Requête

| Endpoint | Nom | Type | Requis | Défaut | Description |
|----------|-----|------|--------|--------|-------------|
| `/achievementprogress` | take | int | Non | 10 | Nombre maximum de packages à retourner |

---

## `GET /sbox/player/{steamId}`

### Exemple de Requête
`GET /sbox/player/76561197960279927`

### Exemple de Réponse
```json
{
  "Id": 76561197960279927,
  "Name": "garry",
  "Url": "/u/garry",
  "Avatar": "https://avatars.steamstatic.com/adfd2545a3a5f0eb07ad042873be6aa4e03ef660_medium.jpg",
  "Online": true,
  "Score": 1375
}
```

### Champs de Réponse
| Champ   | Type    | Description |
|---------|---------|-------------|
| Id      | long    | Steam ID du joueur (64-bit) |
| Name    | string  | Nom d'affichage |
| Url     | string  | URL relative du profil (`/u/...`) |
| Avatar  | string  | URL de l'avatar Steam |
| Online  | bool    | Indique si le joueur est actuellement actif sur S&box (parfois absent quand faux) |
| Private | bool    | Indique un profil privé (absent quand faux) |
| Score   | int     | Score de réputation / d'activité |

---

## `GET /sbox/player/{steamId}/overview`

Agrégats globaux et activité récente du joueur.

### Exemple de Réponse (tronquée)
```json
{
  "Player": { "Id": 76561197960279927, "Name": "garry", "Score": 1375 },
  "GamesPlayed": 341,
  "TotalSessions": 3028,
  "SecondsPlayed": 548335,
  "Achievements": 60,
  "TotalFavourites": 68,
  "TotalReviews": 33,
  "NegativeReviews": 3,
  "PositiveReviews": 15,
  "Avatar": "{\"Items\":[...],\"Height\":0.62,\"DisplayName\":\"Garry\"}",
  "LatestReviews": [ /* PackageReviewDto[] */ ],
  "MostPlayed": { /* PackageWrapMinimal */ },
  "LatestPlayed": { /* PackageWrapMinimal */ }
}
```

### Champs de Réponse (`PlayerOverview`)
| Champ           | Type    | Description |
|-----------------|---------|-------------|
| Player          | object  | Profil de base (même structure que l'endpoint racine) |
| GamesPlayed     | long    | Nombre de jeux distincts lancés |
| TotalSessions   | long    | Total des sessions sur tous les packages |
| SecondsPlayed   | long    | Temps de jeu cumulé en secondes |
| Achievements    | long    | Succès débloqués |
| TotalFavourites | long    | Packages favoris du joueur |
| TotalReviews    | long    | Avis publiés par ce joueur |
| NegativeReviews | long    | Sous-ensemble d'avis négatifs |
| PositiveReviews | long    | Sous-ensemble d'avis positifs |
| Avatar          | string  | Configuration Citizen encodée en JSON (vêtements, taille, nom…) — à parser côté client |
| LatestReviews   | array   | Avis les plus récents (`PackageReviewDto[]`) |
| MostPlayed      | object  | Package avec le plus de temps de jeu (`PackageWrapMinimal`) |
| LatestPlayed    | object  | Package joué le plus récemment (`PackageWrapMinimal`) |
| TopPlayed       | array   | Top des packages joués avec `SecondsPlayed`, `AchUnlocked`, `LastSeen` |
| RecentlyPlayed  | array   | Packages récemment joués (même structure que `TopPlayed`) |

> Le champ `Avatar` est une **chaîne** contenant du JSON. Elle encode la tenue Citizen du joueur ; il faut faire `JSON.parse` avant de lire les champs imbriqués.

---

## `GET /sbox/player/{steamId}/achievementprogress`

Progression des succès du joueur, package par package.

### Exemple de Réponse (tronquée)
```json
[
  {
    "Package": { /* PackageWrapMinimal */ },
    "Achievements": [ /* AchievementDto[] */ ],
    "LastSeen": "2026-05-18T19:12:57.7+00:00",
    "Unlocked": 3,
    "Score": 80,
    "Total": 12,
    "TotalScore": 500
  }
]
```

### Champs de Réponse (`PlayerAchievementProgress`)
| Champ        | Type   | Description |
|--------------|--------|-------------|
| Package      | object | Package minimal |
| Achievements | array  | Entrées par succès (voir l'endpoint Succès) |
| LastSeen     | string | Dernière fois où le joueur a lancé ce package (ISO 8601) |
| Unlocked     | int    | Nombre de succès débloqués par le joueur |
| Score        | int    | Somme des `Score` des succès débloqués |
| Total        | int    | Nombre total de succès dans le package |
| TotalScore   | int    | Somme des `Score` de tous les succès du package |

## Réponses d'Erreur

| Code Statut | Description |
|-------------|-------------|
| 404 | Joueur introuvable |
| 400 | Format de Steam ID invalide |
| 429 | Limite de taux dépassée |

## Notes

- Les Steam IDs doivent être en format 64-bit (17 chiffres, préfixés `7656`).
- Le champ `Url` est relatif : préfixez-le avec `https://sbox.game` pour obtenir l'URL complète.
- Les champs `Online` et `Private` sont absents de la réponse lorsque leur valeur est `false`.
- Utilisez le paramètre `take` sur `/achievementprogress` pour limiter le nombre de packages retournés (défaut : 10).
