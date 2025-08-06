# Actualités d'Organisation

## Endpoint
`GET https://services.facepunch.com/sbox/news/organization/{orgIdent}`

## Description
Récupérer les articles d'actualités et mises à jour publiés par une organisation spécifique, incluant les annonces, mises à jour de packages et autres contenus de l'organisation.

## Paramètres de Chemin

| Nom      | Type   | Requis | Description |
|----------|--------|--------|-------------|
| orgIdent | string | Oui    | L'identifiant de l'organisation |

---

## Exemple de Requête
`GET /sbox/news/organization/smallboxstudio`

---

## Exemple de Réponse
```json
[
  {
    "Id": "10c7ac25-2c71-4256-b7c9-1de99407ca33",
    "Created": "2025-07-09T09:11:24.6+00:00",
    "Title": "Update 1.0",
    "Summary": "",
    "Url": "/smallboxstudio/rpunioncity/news/update-10-e02a59c0",
    "Author": {
      "Id": 76561198050191277,
      "Name": "TwinsD",
      "Url": "/u/SecretDefense",
      "Avatar": "https://avatars.steamstatic.com/d3bb2c897bc789bcc3d8e2a459820d9f01798f9a_medium.jpg",
      "Score": 4549
    },
    "Package": "smallboxstudio.rpunioncity",
    "Media": "https://cdn.sbox.game/upload/i/a6be19b2/630f/485d/b957/d9045d282820",
    "Sections": [
      {
        "Id": "9a728432-20c6-419b-8fe6-2a1b02d6e0d3",
        "Title": "update 1.0",
        "Author": {
          "Id": 76561198050191277,
          "Name": "TwinsD",
          "Url": "/u/SecretDefense",
          "Avatar": "https://avatars.steamstatic.com/d3bb2c897bc789bcc3d8e2a459820d9f01798f9a_medium.jpg",
          "Score": 4549
        },
        "SortOrder": 5000,
        "Contents": "<div><!--block-->– Ajout d'éclairages.<figure data-trix-attachment=\"...\">...</figure>– Suppression des props inutiles.<br><br>– Suppression des décalcomanies inutiles.<br><br>– Ajout de props physiques.<figure data-trix-attachment=\"...\">...</figure>– Optimisation de la carte suite à la mise à jour Hammer.</div>",
        "Slug": "update-10"
      }
    ]
  }
]
```

## Champs de Réponse

### Informations de l'Article
| Champ   | Type   | Description |
|---------|--------|-------------|
| Id      | string | Identifiant unique de l'article (UUID) |
| Created | string | Horodatage de création de l'article |
| Title   | string | Titre de l'article |
| Summary | string | Résumé/extrait de l'article |
| Url     | string | URL relative vers l'article complet |
| Package | string | Identifiant du package associé (format: org.package) |
| Media   | string | URL du média en vedette pour l'article |

### Informations de l'Auteur
| Champ         | Type   | Description |
|---------------|--------|-------------|
| Author.Id     | long   | ID Steam de l'auteur |
| Author.Name   | string | Nom d'affichage de l'auteur |
| Author.Url    | string | URL relative vers le profil de l'auteur |
| Author.Avatar | string | URL de l'image d'avatar de l'auteur |
| Author.Score  | int    | Score communautaire de l'auteur |

### Sections de l'Article
| Champ                | Type   | Description |
|---------------------|--------|-------------|
| Sections[].Id       | string | Identifiant unique de la section |
| Sections[].Title    | string | Titre de la section |
| Sections[].Author   | object | Auteur de la section (même structure que l'auteur principal) |
| Sections[].SortOrder| int    | Ordre d'affichage de la section |
| Sections[].Contents | string | Contenu de la section (format HTML) |
| Sections[].Slug     | string | Identifiant de section compatible URL |

## Format du Contenu

Le champ `Contents` contient du contenu HTML riche qui peut inclure :
- Formatage de texte (gras, italique, etc.)
- Images avec métadonnées (`data-trix-attachment`)
- Vidéos et autres fichiers multimédias
- Liens et références
- Pièces jointes avec informations de taille

## Réponses d'Erreur

| Code Statut | Description |
|-------------|-------------|
| 404         | Organisation introuvable |
| 400         | Identifiant d'organisation invalide |
| 429         | Limite de taux dépassée |

## Notes

- La réponse est un tableau d'articles d'actualités, classés par date de création (plus récent en premier)
- Les articles peuvent être associés à des packages spécifiques via le champ `Package`
- Le champ `Contents` utilise le format HTML de l'éditeur Trix avec métadonnées multimédias intégrées
- Les URL de médias pointent vers le CDN de S&box pour les images, vidéos et autres pièces jointes
- Certains articles peuvent avoir des champs `Summary` vides
- `SortOrder` détermine la séquence d'affichage des