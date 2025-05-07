# BOCM-API-documentation
Documentación de API del Catálogo de Datos Abiertos datos.comunidad.madrid/api
No existe una documentación oficial, creo este repo como guia para usarla.

## Introducción

El Boletín Oficial de la Comunidad de Madrid (BOCM) cuenta con un API pública para la reutilización de datos, accesible a través de la plataforma CKAN en el portal de datos abiertos de la Comunidad de Madrid. Este API REST estándar permite listar, buscar y consultar datasets y recursos asociados, incluidos aquellos publicados en el BOCM. La autenticación no es necesaria para operaciones de solo lectura, aunque ciertos endpoints de escritura requieren un token de API. ([comunidad.madrid](https://www.comunidad.madrid/gobierno/datos-abiertos/reutiliza-datos-abiertos-comunidad-madrid), [docs.ckan.org](https://docs.ckan.org/en/2.7/api/index.html))

## Autenticación

* **Operaciones de solo lectura**: No requieren autenticación.
* **Operaciones de escritura** (create, update, delete): Requieren un `API Key` obtenido al registrarse como reutilizador en el portal de datos abiertos. ([docs.ckan.org](https://docs.ckan.org/en/2.7/api/index.html))

## Base URL

```
https://datos.comunidad.madrid/api/3/action
```

※ Versionado explícito de la API (`/3/`) recomendado para estabilidad de cliente. ([docs.ckan.org](https://docs.ckan.org/en/2.7/api/index.html))

## Endpoints principales

### 1. Listado de Datasets

```
GET /package_list
```

Devuelve un array con los identificadores de todos los datasets disponibles. ([comunidad.madrid](https://www.comunidad.madrid/gobierno/datos-abiertos/reutiliza-datos-abiertos-comunidad-madrid))

### 2. Búsqueda de Datasets

```
GET /package_search?q=<término>&rows=<n>&start=<offset>
```

Permite buscar datasets por palabra clave, paginación (`rows`, `start`) y orden. Ejemplo:

```
GET https://datos.comunidad.madrid/api/3/action/package_search?q=bocm
```

([comunidad.madrid](https://www.comunidad.madrid/gobierno/datos-abiertos/reutiliza-datos-abiertos-comunidad-madrid))

### 3. Mostrar Detalle de un Dataset

```
GET /package_show?id=<dataset_id>
```

Muestra metadatos completos y lista de recursos de un dataset. Ejemplo para COVID-19 TIA:

```
GET https://datos.comunidad.madrid/api/3/action/package_show?id=covid19_tia_zonas_basicas_salud
```

([datos.comunidad.madrid](https://datos.comunidad.madrid/dataset/covid19_tia_zonas_basicas_salud?utm_source=chatgpt.com))

### 4. Listado de Grupos

```
GET /group_list
```

Obtiene todos los grupos (vocabularios temáticos) disponibles. ([comunidad.madrid](https://www.comunidad.madrid/gobierno/datos-abiertos/reutiliza-datos-abiertos-comunidad-madrid))

### 5. Listado de Etiquetas

```
GET /tag_list
```

Devuelve todas las etiquetas utilizadas en los datasets. ([comunidad.madrid](https://www.comunidad.madrid/gobierno/datos-abiertos/reutiliza-datos-abiertos-comunidad-madrid))

### 6. Mostrar Recurso (Resource)

```
GET /resource_show?id=<resource_id>
```

Recupera detalles de un recurso específico (URL de descarga, formato, etc.). Ejemplo:

```
GET https://datos.comunidad.madrid/api/3/action/resource_show?id=105a2d74-6507-4ce5-9b85-a17acba9b143
```

([datos.comunidad.madrid](https://datos.comunidad.madrid/dataset/0fb0c380-477c-4396-b2d1-f0f2ed90bb38/resource/105a2d74-6507-4ce5-9b85-a17acba9b143?utm_source=chatgpt.com))

### 7. Actividad Reciente

```
GET /recently_changed_packages_activity_list
```

Obtiene un feed de datasets modificados recientemente. ([comunidad.madrid](https://www.comunidad.madrid/gobierno/datos-abiertos/reutiliza-datos-abiertos-comunidad-madrid))

## Ejemplos de uso con cURL

* Listar 5 primeros datasets:

  ```bash
  curl "https://datos.comunidad.madrid/api/3/action/package_list?limit=5"
  ```

* Buscar datasets relacionados con "luz":

  ```bash
  curl "https://datos.comunidad.madrid/api/3/action/package_search?q=luz&rows=10"
  ```

* Ver detalle de un dataset:

  ```bash
  curl "https://datos.comunidad.madrid/api/3/action/package_show?id=municipio_comunidad_madrid"
  ```

## Descarga directa de recursos

Una vez conocida la URL de descarga de un recurso (campo `url` en `resource_show`), basta con realizar un `GET` estándar:

```
GET <url_del_recurso>
```

Ejemplo CSV COVID-19 TIA:

````
https://datos.comunidad.madrid/catalogo/dataset/covid19_tia_zonas_basicas_salud/resource/…/download/covid19_tia_zonas_basicas_salud.csv
``` ([datos.comunidad.madrid](https://datos.comunidad.madrid/dataset/0fb0c380-477c-4396-b2d1-f0f2ed90bb38/resource/105a2d74-6507-4ce5-9b85-a17acba9b143?utm_source=chatgpt.com))

## Más información

- Documentación completa del API CKAN: https://docs.ckan.org/en/2.7/api/index.html ([docs.ckan.org](https://docs.ckan.org/en/2.7/api/index.html))
- Portal reutilización de datos abiertos Comunidad de Madrid: https://www.comunidad.madrid/datos-abiertos ([comunidad.madrid](https://www.comunidad.madrid/gobierno/datos-abiertos/reutiliza-datos-abiertos-comunidad-madrid))

````
