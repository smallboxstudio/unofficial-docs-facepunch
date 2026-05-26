# Statistiques de Package

## Endpoints

| Méthode | Chemin |
|---------|--------|
| GET | `https://services.facepunch.com/sbox/package/stats/2/{packageIdent}` |
| GET | `https://services.facepunch.com/sbox/package/stats/2/{packageIdent}/u/{steamId}` |

## Description
Récupérer les statistiques de jeu publiées par un package. Le premier endpoint retourne les agrégats globaux pour chaque statistique déclarée par le package. Le second retourne les valeurs par joueur pour un Steam ID donné.

## Paramètres de Chemin

| Nom          | Type   | Requis | Description |
|--------------|--------|--------|-------------|
| packageIdent | string | Oui    | Identifiant complet du package (`org.package`) |
| steamId      | long   | Oui (utilisateur) | Steam ID du joueur dont on veut les statistiques |

---

## Exemples de Requête
```
GET /sbox/package/stats/2/facepunch.testbed
GET /sbox/package/stats/2/facepunch.testbed/u/76561197960279927
```

---

## Exemple de Réponse (global)
```json
[
  {
    "Name": "balls_fired",
    "Title": "balls_fired",
    "Description": "",
    "Unit": "",
    "Value": 1,
    "ValueString": "1",
    "Players": 8109,
    "Max": 1,
    "Avg": 1,
    "Min": 1,
    "Sum": 8897209
  }
]
```

## Champs de Réponse

### `GlobalStat`
| Champ        | Type   | Description |
|--------------|--------|-------------|
| Name         | string | Identifiant de la statistique |
| Title        | string | Titre d'affichage |
| Description  | string | Description optionnelle |
| Unit         | string | Unité (ex. "kills", "secondes") |
| Velocity     | number | Indicateur de variation |
| Value        | number | Valeur de référence |
| ValueString  | string | Valeur d'affichage pré-formatée |
| Players      | int    | Nombre de joueurs ayant contribué |
| Max          | number | Valeur individuelle maximale |
| Avg          | number | Moyenne |
| Min          | number | Valeur individuelle minimale |
| Sum          | number | Somme sur tous les joueurs |

### `PlayerStat` (endpoint par utilisateur)
| Champ        | Type   | Description |
|--------------|--------|-------------|
| Name         | string | Identifiant de la statistique |
| Title        | string | Titre d'affichage |
| Description  | string | Description optionnelle |
| Unit         | string | Unité |
| Value        | number | Valeur actuelle du joueur |
| ValueString  | string | Valeur d'affichage pré-formatée |
| Max / Min / Avg / Sum | number | Agrégats par joueur |
| First        | string | Date de la première contribution (ISO 8601) |
| FirstValue   | number | Valeur lors de la première contribution |
| Last         | string | Date de la contribution la plus récente (ISO 8601) |
| LastValue    | number | Valeur lors de la dernière contribution |

## Réponses d'Erreur

| Code Statut | Description |
|-------------|-------------|
| 404 | Package ou joueur introuvable |
| 400 | Identifiant invalide |
| 429 | Limite de taux dépassée |

## Notes

- Retourne un tableau vide si le package ne déclare aucune statistique ou si le joueur n'en a pas enregistrée.
- `Velocity` n'est rempli que sur les statistiques globales.
- Les noms de statistiques sont des chaînes libres définies par chaque package.
