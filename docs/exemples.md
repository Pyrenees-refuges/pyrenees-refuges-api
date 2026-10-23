# Exemples d’utilisation de l’API Pyrénées Refuges

Ces exemples récupèrent dix lieux au format GeoJSON et affichent leur nom ainsi que leurs coordonnées.

Documentation complète : https://www.pyrenees-refuges.com/api/

Conditions de réutilisation : https://www.pyrenees-refuges.com/api/licence/

## PHP

Prérequis : PHP 8 avec l’extension cURL.

Enregistrer ce code dans un fichier `exemple.php`, puis l’exécuter avec PHP.

```php
<?php
declare(strict_types=1);

$url = 'https://www.pyrenees-refuges.com/api.php?format=geojson&limit=10';
$curl = curl_init($url);

if ($curl === false) {
    exit("Impossible d’initialiser cURL.\n");
}

curl_setopt_array($curl, [
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_CONNECTTIMEOUT => 10,
    CURLOPT_TIMEOUT => 20,
    CURLOPT_HTTPHEADER => ['Accept: application/geo+json'],
]);

try {
    // Télécharger la réponse et vérifier le statut HTTP.
    $response = curl_exec($curl);

    if ($response === false) {
        throw new RuntimeException('Erreur réseau : ' . curl_error($curl));
    }

    $status = (int) curl_getinfo($curl, CURLINFO_HTTP_CODE);

    if ($status !== 200) {
        throw new RuntimeException('Erreur HTTP : ' . $status);
    }

    $data = json_decode($response, true, 512, JSON_THROW_ON_ERROR);

    if (($data['type'] ?? '') !== 'FeatureCollection'
        || !is_array($data['features'] ?? null)) {
        throw new RuntimeException('Réponse GeoJSON inattendue.');
    }

    $lines = [];

    foreach ($data['features'] as $feature) {
        $name = $feature['properties']['name'] ?? 'Sans nom';
        $coordinates = $feature['geometry']['coordinates'] ?? [];

        if (count($coordinates) < 2) {
            continue;
        }

        // GeoJSON utilise l’ordre longitude, latitude.
        [$longitude, $latitude] = $coordinates;
        $lines[] = $name . ' — latitude : ' . $latitude
            . ', longitude : ' . $longitude;
    }

    $output = implode("\n", $lines);

    // Échapper les textes si cet exemple est affiché dans une page web.
    echo PHP_SAPI === 'cli'
        ? $output . "\n"
        : '<pre>' . htmlspecialchars(
            $output,
            ENT_QUOTES | ENT_SUBSTITUTE,
            'UTF-8'
        ) . '</pre>';
} catch (Throwable $error) {
    echo PHP_SAPI === 'cli'
        ? $error->getMessage() . "\n"
        : htmlspecialchars(
            $error->getMessage(),
            ENT_QUOTES | ENT_SUBSTITUTE,
            'UTF-8'
        );
}
```

## JavaScript

Cet exemple utilise `fetch` et affiche les résultats dans la console.

Il peut être exécuté avec Node.js 18 ou une version ultérieure. Dans un navigateur, les appels depuis un autre domaine nécessitent que le serveur autorise cet accès par sa configuration CORS.

```javascript
async function afficherLieux() {
    const url = 'https://www.pyrenees-refuges.com/api.php?format=geojson&limit=10';
    const controller = new AbortController();
    const timeout = setTimeout(() => controller.abort(), 20000);

    try {
        // Télécharger les données, avec un délai maximal de 20 secondes.
        const response = await fetch(url, {
            signal: controller.signal
        });

        if (!response.ok) {
            throw new Error('Erreur HTTP : ' + response.status);
        }

        const data = await response.json();

        if (data.type !== 'FeatureCollection' || !Array.isArray(data.features)) {
            throw new Error('Réponse GeoJSON inattendue.');
        }

        for (const feature of data.features) {
            const coordinates = feature.geometry?.coordinates;

            if (!Array.isArray(coordinates) || coordinates.length < 2) {
                continue;
            }

            const name = feature.properties?.name ?? 'Sans nom';
            const [longitude, latitude] = coordinates;

            console.log(`${name} — latitude : ${latitude}, longitude : ${longitude}`);
        }
    } catch (error) {
        console.error('Impossible de charger les lieux :', error.message);
    } finally {
        clearTimeout(timeout);
    }
}

afficherLieux();
```

## Python

Prérequis : Python 3. Aucune bibliothèque supplémentaire à installer.

Enregistrer ce code dans un fichier `exemple.py`, puis l’exécuter avec Python.

```python
import json
import urllib.error
import urllib.request

URL = "https://www.pyrenees-refuges.com/api.php?format=geojson&limit=10"

try:
    # Télécharger les données avec un délai maximal de 20 secondes.
    request = urllib.request.Request(
        URL,
        headers={"Accept": "application/geo+json"},
    )

    with urllib.request.urlopen(request, timeout=20) as response:
        data = json.load(response)

    if (
        not isinstance(data, dict)
        or data.get("type") != "FeatureCollection"
        or not isinstance(data.get("features"), list)
    ):
        raise ValueError("Réponse GeoJSON inattendue.")

    for feature in data["features"]:
        properties = feature.get("properties") or {}
        geometry = feature.get("geometry") or {}
        coordinates = geometry.get("coordinates") or []

        if len(coordinates) < 2:
            continue

        name = properties.get("name", "Sans nom")

        # GeoJSON utilise l’ordre longitude, latitude.
        longitude, latitude = coordinates[:2]

        print(f"{name} — latitude : {latitude}, longitude : {longitude}")

except urllib.error.HTTPError as error:
    print(f"Erreur HTTP : {error.code}")
except (urllib.error.URLError, TimeoutError, OSError) as error:
    print(f"Erreur réseau : {error}")
except (ValueError, TypeError, AttributeError) as error:
    print(f"Réponse impossible à traiter : {error}")
```

## Adapter la recherche

Modifier les paramètres de l’URL pour :

- sélectionner un format avec `format` ;
- limiter une zone avec `bbox=ouest,sud,est,nord` ;
- sélectionner une catégorie avec `type_lieu` ;
- paginer avec `limit` et `offset`.

Consulter la documentation officielle avant de modifier ces paramètres.

## Utilisation raisonnable

Conserver temporairement les résultats lorsque cela est possible et éviter de répéter inutilement la même requête.

Les conditions de réutilisation des données restent applicables aux résultats obtenus avec ces exemples.
