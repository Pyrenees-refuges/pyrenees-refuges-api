# API Pyrénées Refuges

Documentation de l’API de [Pyrénées Refuges](https://www.pyrenees-refuges.com), site participatif consacré aux refuges, cabanes et abris des Pyrénées.

Cette API publique en lecture seule permet de récupérer des données géographiques pour des projets cartographiques et des applications de randonnée.

## Liens utiles

- [Site Pyrénées Refuges](https://www.pyrenees-refuges.com/)
- [Documentation de l’API](https://www.pyrenees-refuges.com/api/)
- [Générateur interactif de requêtes](https://www.pyrenees-refuges.com/api-builder.php)
- [Description OpenAPI](https://www.pyrenees-refuges.com/openapi.json)
- [Conditions de réutilisation des données](https://www.pyrenees-refuges.com/api/licence/)
- [Exemples PHP, JavaScript et Python](docs/exemples.md)
- 
## Première requête

Obtenir dix lieux au format GeoJSON :

https://www.pyrenees-refuges.com/api.php?format=geojson&limit=10

## Fonctionnement

- Méthode HTTP : GET
- Point d’accès : https://www.pyrenees-refuges.com/api.php
- Formats : GeoJSON, JSON, CSV, GPX et KML
- Coordonnées : WGS84 — EPSG:4326
- Ordre des coordonnées géographiques : longitude, latitude

## Paramètres

| Paramètre | Description | Exemple |
|---|---|---|
| `format` | Format de sortie : `geojson`, `json`, `csv`, `gpx` ou `kml` | `format=geojson` |
| `bbox` | Emprise géographique : ouest,sud,est,nord | `bbox=-1,42,1,43` |
| `type_lieu` | Catégorie de lieu : `all` ou numéro de 1 à 7 | `type_lieu=2` |
| `region` | Nom exact de la région enregistrée | Utiliser le générateur interactif |
| `limit` | Nombre maximal de résultats | `limit=100` |
| `offset` | Décalage pour la pagination | `offset=100` |

Pour les valeurs par défaut et les limites applicables, consulter la documentation officielle.

## Catégories de lieux

| Code | Catégorie |
|---|---|
| 1 | Cabane fermée |
| 2 | Cabane ouverte |
| 3 | Cabane occupée par un berger en été |
| 4 | Orri, toue ou abri en pierre |
| 5 | Refuge gardé l’été |
| 6 | Refuge gardé toute l’année |
| 7 | Ruine |

## Exemples de requêtes

### Export GeoJSON limité à une zone

https://www.pyrenees-refuges.com/api.php?format=geojson&bbox=-1,42,1,43&limit=100

### Dix lieux au format JSON

https://www.pyrenees-refuges.com/api.php?format=json&limit=10

### Cent cabanes ouvertes au format GeoJSON

https://www.pyrenees-refuges.com/api.php?format=geojson&type_lieu=2&limit=100

### Export GPX de dix lieux

https://www.pyrenees-refuges.com/api.php?format=gpx&limit=10

### Pagination

Première page :

https://www.pyrenees-refuges.com/api.php?format=geojson&limit=100&offset=0

Page suivante :

https://www.pyrenees-refuges.com/api.php?format=geojson&limit=100&offset=100

## Bonnes pratiques

- Limiter les résultats aux besoins du projet.
- Privilégier un filtre géographique pour les recherches locales.
- Conserver temporairement les réponses afin d’éviter les téléchargements répétés.
- Prévoir la gestion des erreurs et des indisponibilités.
- Consulter les conditions de réutilisation avant toute redistribution.

## Réutilisation des données

Les conditions de référence sont publiées sur la page suivante :

https://www.pyrenees-refuges.com/api/licence/

La licence CC BY-NC 4.0 concerne uniquement les données pour lesquelles Pyrénées Refuges détient les droits nécessaires, selon les exclusions précisées sur cette page.

Les photographies, fonds cartographiques et contenus de tiers ne sont pas automatiquement couverts.

Les conditions applicables aux données sont distinctes de celles des éventuels fichiers de code publiés dans ce dépôt.

## Signaler un problème

Pour un problème concernant la documentation ou l’utilisation de l’API, ouvrir une discussion dans l’onglet **Issues** de ce dépôt.

Pour proposer une correction sur un lieu, utiliser le bouton **Modifier** de sa fiche sur Pyrénées Refuges.

Ne jamais publier de mot de passe, de clé API ou d’information personnelle dans une demande.

## English

Pyrénées Refuges provides a public, read-only API for geographical information about mountain huts, shelters and refuges in the Pyrenees.

Supported formats: GeoJSON, JSON, CSV, GPX and KML.

- [English documentation](https://www.pyrenees-refuges.com/en/api/)
- [Request builder](https://www.pyrenees-refuges.com/api-builder.php)
- [Data reuse conditions](https://www.pyrenees-refuges.com/en/api/licence/)

Please review the data reuse conditions before using or redistributing the results.
