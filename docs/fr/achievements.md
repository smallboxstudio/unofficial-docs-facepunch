# Succès

## Endpoint
`GET https://services.facepunch.com/sbox/achievement/list?package={packageIdent}`

## Description
Lister tous les succès déclarés par un package S&box : conditions de déblocage, fraction globale de joueurs ayant débloqué, icônes, points attribués.

## Paramètres de Requête

| Nom     | Type   | Requis | Description |
|---------|--------|--------|-------------|
| package | string | Oui    | Identifiant complet du package (`org.package`) |

---

## Exemple de Requête
`GET /sbox/achievement/list?package=facepunch.testbed`

---

## Exemple de Réponse
```json
[
  {
    "Name": "100_cubes",
    "Title": "100 Cubes",
    "Description": "You have shot 100 cubes",
    "Icon": "https://cdn.sbox.game/upload/i/a4db95ca/a79a/46f8/a87c/fa37c3516a71.png",
    "SourceStat": "cubes_fired",
    "Max": 100,
    "ShowProgress": true,
    "UnlockMode": 1,
    "Score": 50,
    "GlobalUnlocks": 251,
    "GlobalFraction": 0.0118
  }
]
```

## Champs de Réponse (`AchievementDto`)

| Champ             | Type    | Description |
|-------------------|---------|-------------|
| Name              | string  | Identifiant interne stable |
| Title             | string  | Titre d'affichage |
| Description       | string  | Description d'affichage |
| Icon              | string  | URL CDN de l'icône |
| Visibility        | enum    | `Visible`, `VisibleWhenUnlocked`, `Hidden` |
| SourceStat        | string  | Statistique qui pilote le déblocage automatique (si `UnlockMode = Stat`) |
| SourceAggregation | enum    | Agrégation appliquée à `SourceStat` (`Sum`, `Max`, `Min`, `Last`, `Avg`) |
| Min               | number  | Valeur minimale pour la progression |
| Max               | number  | Valeur de stat requise pour débloquer |
| ShowProgress      | bool    | Indique si le client doit afficher une barre de progression |
| UnlockMode        | enum    | `0` = Manuel, `1` = Stat |
| Score             | int     | Points attribués au déblocage |
| GlobalUnlocks     | int     | Nombre de joueurs ayant débloqué globalement |
| GlobalFraction    | number  | Fraction de joueurs ayant débloqué (0–1) |
| Unlocked          | string? | Si l'appel est dans un contexte utilisateur, date de déblocage (ISO 8601) — sinon absent |

## Réponses d'Erreur

| Code Statut | Description |
|-------------|-------------|
| 404 | Package introuvable |
| 400 | Paramètre `package` manquant ou invalide |
| 429 | Limite de taux dépassée |

## Notes

- Les succès sont retournés dans l'ordre défini par le package.
- `GlobalUnlocks` et `GlobalFraction` sont recalculés périodiquement et peuvent être légèrement en retard.
- La mutation `POST /sbox/achievement/unlock` requiert une authentification et ne fait pas partie de la surface publique en lecture.
