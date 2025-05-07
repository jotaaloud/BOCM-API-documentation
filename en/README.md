## Introduction
There is no official documentation for the public API of the BOCM (Official Gazette of the Community of Madrid), so I created this repository as a usage guide.  
The Official Gazette of the Community of Madrid (BOCM) provides a public API for data reuse, accessible through the CKAN platform on the Community of Madrid's Open Data Portal.  This standard REST API allows listing, searching, and querying datasets and associated resources, including those published in the BOCM. Authentication is not required for read-only operations, although some write endpoints require an API token. ([comunidad.madrid](https://www.comunidad.madrid/gobierno/datos-abiertos/reutiliza-datos-abiertos-comunidad-madrid), [docs.ckan.org](https://docs.ckan.org/en/2.7/api/index.html))

## Authentication

* **Read-only operations**: Do not require authentication.
* **Write operations** (create, update, delete): Require an `API Key` obtained by registering as a data reuser on the open data portal. ([docs.ckan.org](https://docs.ckan.org/en/2.7/api/index.html))

## Base URL

```
https://datos.comunidad.madrid/api/3/action
```

※ Explicit API versioning (`/3/`) is recommended for client-side stability.

## Main Endpoints

### 1. Dataset List

```
GET /package_list
```

Returns an array of all available dataset identifiers.

### 2. Dataset Search

```
GET /package_search?q=<term>&rows=<n>&start=<offset>
```

Search for datasets by keyword, with pagination and ordering. Example:

```
GET https://datos.comunidad.madrid/api/3/action/package_search?q=bocm
```

### 3. Dataset Details

```
GET /package_show?id=<dataset_id>
```

Returns full metadata and resource list for a dataset. Example:

```
GET https://datos.comunidad.madrid/api/3/action/package_show?id=covid19_tia_zonas_basicas_salud
```

### 4. Group List

```
GET /group_list
```

Fetches all available groups (thematic vocabularies).

### 5. Tag List

```
GET /tag_list
```

Returns all tags used in datasets.

### 6. Resource Details

```
GET /resource_show?id=<resource_id>
```

Returns details of a specific resource (download URL, format, etc.). Example:

```
GET https://datos.comunidad.madrid/api/3/action/resource_show?id=105a2d74-6507-4ce5-9b85-a17acba9b143
```

### 7. Recently Changed Packages

```
GET /recently_changed_packages_activity_list
```

Returns a feed of recently modified datasets.

## cURL Usage Examples

* List first 5 datasets:

  ```bash
  curl "https://datos.comunidad.madrid/api/3/action/package_list?limit=5"
  ```

* Search datasets related to "luz" (light):

  ```bash
  curl "https://datos.comunidad.madrid/api/3/action/package_search?q=luz&rows=10"
  ```

* Get dataset details:

  ```bash
  curl "https://datos.comunidad.madrid/api/3/action/package_show?id=municipio_comunidad_madrid"
  ```

## Direct Resource Download

Once you retrieve the resource download URL (`url` field in `resource_show`), use a standard `GET` request:

```
GET <resource_url>
```

Example CSV file for COVID-19 TIA:

```
https://datos.comunidad.madrid/catalogo/dataset/covid19_tia_zonas_basicas_salud/resource/…/download/covid19_tia_zonas_basicas_salud.csv
```

## More Information

* Full CKAN API documentation: [https://docs.ckan.org/en/2.7/api/index.html](https://docs.ckan.org/en/2.7/api/index.html)
* Open Data Portal of the Community of Madrid: [https://www.comunidad.madrid/datos-abiertos](https://www.comunidad.madrid/datos-abiertos)
