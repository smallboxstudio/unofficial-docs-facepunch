# Informations Joueur

## Endpoint
`GET https://services.facepunch.com/sbox/player/{steamId}`

## Description
Récupérer des informations détaillées sur un joueur S&box spécifique en utilisant son Steam ID, incluant les données de profil et le score.

## Paramètres de Chemin

| Nom     | Type   | Requis | Description |
|---------|--------|--------|-------------|
| steamId | string | Oui    | L'ID Steam du joueur (format 64-bit). |

---

## Exemple de Requête
`GET /sbox/player/76561197960279927`

---

## Exemple de Réponse
```json
{
  "Id": 76561197960279927,
  "Name": "garry",
  "Url": "/u/garry",
  "Avatar": "https://avatars.steamstatic.com/adfd2545a3a5f0eb07ad042873be6aa4e03ef660_medium.jpg",
  "Online":true,
  "Score": 1200
}
```

## Champs de Réponse

| Champ  | Type   | Description |
|--------|--------|-------------|
| Id     | long   | L'ID Steam du joueur (format 64-bit) |
| Name   | string | Nom d'affichage du joueur |
| Url    | string | URL relative vers la page de profil du joueur |
| Avatar | string | URL vers l'image d'avatar Steam du joueur |
| Online | boolean | Indique si le joueur est actuellement en ligne |
| Score  | int    | Score/réputation actuel du joueur |

## Réponses d'Erreur

| Code Statut | Description |
|-------------|-------------|
| 404         | Joueur introuvable ou ID Steam invalide |
| 400         | Format d'ID Steam invalide |
| 429         | Limite de taux dépassée |

## Notes

- Les ID Steam doivent être au format 64-bit (nombre à 17 chiffres commençant par 765...)
- Le champ `Url` fournit un chemin relatif qui peut être ajouté à l'URL de base du site S&box
- Les images d'avatar proviennent du CDN de Steam
- Le champ `Online` indique si le joueur est actuellement actif dans S&box il apparaît pas si le joueur n'est pas en ligne
- Le Score représente la réputation ou le niveau d'activité du joueur dans la communauté S&box