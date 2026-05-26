# Recherche de Packages

## Endpoint
`GET https://services.facepunch.com/sbox/package/find/2`

> L'ancien endpoint `/sbox/package/find` pointe vers le même handler.

## Description
Rechercher des packages (maps, gamemodes, jeux, assets…) sur la plateforme S&box avec filtrage, tri et pagination. La requête est exprimée par une chaîne unique avec des préfixes (`type:`, `sort:`, `+tag`, `org:`, …).

## Paramètres de Requête

| Nom  | Type   | Requis | Défaut | Description |
|------|--------|--------|--------|-------------|
| q    | string | Oui    | —      | Chaîne de recherche. Supporte les tokens `key:value` (voir ci-dessous). |
| take | int    | Non    | 100    | Nombre maximum de résultats. |
| skip | int    | Non    | 0      | Décalage pour la pagination. |

### Tokens supportés dans `q`

| Forme         | Effet |
|---------------|-------|
| `<texte>`     | Recherche libre dans le titre, résumé et description |
| `+<tag>`      | Exige un tag (plusieurs `+` se combinent en ET logique) |
| `type:<type>` | Filtre par type de package (`game`, `map`, `gamemode`, `tool`, `tutorial`, `clothing`, …) |
| `sort:<ordre>` | Mode de tri (voir table ci-dessous) |
| `org:<ident>` | Restreint à une organisation |
| `asset:<nom>` | Filtre par type d'asset principal |
| `contest:<nom>` | Restreint à un concours |
| `in:<collection>` | Restreint à une collection |
| `target:<pkg>` | Contenu créé pour un package cible |
| `+game:<pkg>` | Contenu créé pour ce jeu (`any` pour effacer) |
| `is:unplayed` | Cache les packages déjà joués (utilisateur authentifié) |
| `is:fave` | Restreint aux favoris de l'utilisateur (authentifié) |

### Valeurs `sort:`

| Valeur | Signification |
|--------|---------------|
| `trending` | Tendance (variante populaire) |
| `popular` | Populaire |
| `newest` / `oldest` | Tri par date de création |
| `updated` | Récemment mis à jour |
| `friends` | Packages joués par des amis (auth) |
| `random` | Aléatoire |
| `upvotes` / `downvotes` | Nombre de votes |
| `favcount` | Nombre de favoris |
| `live`, `referenced`, `referencing`, `user`, `used`, `played` | Récemment utilisés |
| `rankd` / `rankday`, `rankw` / `rankweek`, `rankm` / `rankmonth` | Classement périodique |
| `spawns`, `spawnsday`, `spawnsweek`, `spawnsmonth` | Compte de spawns |
| `playersnow` | Joueurs concurrents en direct |
| `bestrated` / `rated` | Borne inférieure Wilson sur la proportion d'avis |
| `mostreviewed` / `reviewed` | Nombre total d'avis |
| `quality` | Score composite de qualité |
| `hiddengem` / `underrated` | Bien noté mais peu de trafic |

---

## Exemple de Requête
`GET /sbox/package/find/2?q=type:map+sort:trending&take=6&skip=0`

---

## Exemple de Réponse (tronquée)
```json
{
  "Packages": [
    {
      "Org": {
        "Ident": "smallboxstudio",
        "Title": "Small Box Studio",
        "Thumb": "https://cdn.sbox.game/org/smallboxstudio/logo.png",
        "Discord": "https://discord.gg/ucvM2sfTBP"
      },
      "Ident": "spongebobkrustykrab",
      "FullIdent": "smallboxstudio.spongebobkrustykrab",
      "Title": "Sponge Bob Krusty Krab",
      "Summary": "Ohhhh, who lives in a pineapple under the sea ?",
      "Thumb": "https://cdn.sbox.game/.../thumb/...png",
      "TypeName": "map",
      "Updated": "2025-07-21T22:47:55.2+00:00",
      "Created": "2025-06-08T14:06:13.7+00:00",
      "UsageStats": {},
      "Tags": ["map", "ph", "prop", "prophunt"],
      "Favourited": 15,
      "Referencing": 12,
      "VotesUp": 10,
      "Public": true
    }
  ],
  "TotalCount": 1347,
  "Facets": [
    {
      "Name": "category",
      "Title": "Category",
      "Entries": [
        { "Name": "wall", "Title": "Wall", "Icon": "...", "Count": 432, "Children": [] }
      ]
    }
  ],
  "Tags": { "physics": 1820, "multiplayer": 1240 },
  "Orders": [],
  "Properties": []
}
```

## Champs de Réponse

### Racine (`PackageFindResult`)
| Champ      | Type    | Description |
|------------|---------|-------------|
| Packages   | array   | Liste de `PackageWrapMinimal` (voir endpoint Package) |
| TotalCount | long    | Nombre total de packages correspondant |
| Facets     | array   | Filtres facetés pour affiner la recherche |
| Tags       | object  | Map `nomDeTag → compte` pour des suggestions |
| Orders     | array   | Tris disponibles pour la requête active |
| Properties | array   | Tags de propriété mis en avant pour la requête |

### Facette (`PackageFacet`)
| Champ   | Type   | Description |
|---------|--------|-------------|
| Name    | string | Identifiant de la facette |
| Title   | string | Titre d'affichage |
| Entries | array  | Entrées pré-triées — ne pas réordonner côté client |

### Entrée de facette
| Champ    | Type   | Description |
|----------|--------|-------------|
| Name     | string | Token à appliquer pour filtrer (ex. `category:wall`) |
| Title    | string | Titre d'affichage |
| Icon     | string | Icône optionnelle |
| Count    | int    | Nombre de packages dans ce groupe |
| Children | array  | Entrées imbriquées (même structure, récursif) |

## Notes

- Les tokens dans `q` sont séparés par des espaces. Encodez les espaces en `%20` ou `+`.
- Les tokens `key:value` inconnus deviennent automatiquement des filtres de facettes.
- Les champs `Facets`/`Tags`/`Orders`/`Properties` peuvent être vides si la requête ne les demande pas ou si le résultat est trop restreint.
- Pour les détails complets d'un package, appelez ensuite `GET /sbox/package/get/2/{org.package}`.
