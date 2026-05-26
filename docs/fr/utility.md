# Utilitaires

## Endpoint
`GET https://services.facepunch.com/sbox/utility/avatars`

## Description
Renvoie une liste de configurations d'avatars Citizen aléatoires. Utile pour générer des PNJ ou des personnages d'exemple dans un outil. Chaque entrée est une chaîne JSON ayant la même structure que le champ `Avatar` de `/sbox/player/{id}/overview`.

## Requête

Aucun paramètre de chemin ou de requête.

---

## Exemple de Requête
`GET /sbox/utility/avatars`

---

## Exemple de Réponse
```json
[
  "{\"Items\":[{\"p\":\"models/citizen_clothes/body/human03.clothing\"},{\"p\":\"models/citizen_clothes/hat/gasmask/gasmask.clothing\"}],\"Height\":0.58,\"DisplayName\":\"Citizen\",\"Age\":0.41}"
]
```

## Réponse

Retourne `string[]`. Chaque chaîne, une fois parsée, a approximativement cette forme :

| Champ        | Type   | Description |
|--------------|--------|-------------|
| Items        | array  | Vêtements : `{ p: "models/.../<nom>.clothing", t?: <tint> }` |
| Height       | float  | Multiplicateur de taille (0–1) |
| DisplayName  | string | Nom d'affichage aléatoire |
| Age          | float  | Multiplicateur d'âge (0–1) |
| Tint         | float  | Teinte de peau (0–1) |
| PrefersHuman | bool   | Indique si le modèle préfère la base humaine du Citizen |

## Réponses d'Erreur

| Code Statut | Description |
|-------------|-------------|
| 429 | Limite de taux dépassée |
| 500 | Erreur serveur interne |

## Notes

- La réponse est un tableau de **chaînes**, pas d'objets — chaque élément doit être `JSON.parse` séparément.
- C'est le même encodage que le champ `Avatar` des réponses *overview* joueur.
- La taille de la liste est fixée côté serveur ; ne supposez pas un nombre précis.
