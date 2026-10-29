# Estado de estaciones de Ecobici CDMX (octubre de 2026)

Copia **congelada** del estado de las 677 estaciones de Ecobici, capturado cada ~5 minutos del feed GBFS oficial. Es la fuente de datos del challenge técnico de Data Science de Grupo Gigante y **no va a cambiar**.

El feed GBFS de Ecobici solo informa el estado *actual*; no publica historial. Esta serie existe porque [Maximiliano Macías Herrera](https://github.com/Maxtlihuac) la capturó en su proyecto [ecobici-red-cdmx](https://github.com/Maxtlihuac/ecobici-red-cdmx) (rama `gbfs-archive`) y la publicó con licencia MIT. Aquí se redistribuye sin modificar el contenido, conservando su licencia (ver `LICENSE`).

## Periodo

| | |
|---|---|
| Desde | 2026-10-10 00:00 (hora CDMX) |
| Hasta | 2026-10-28 18:00 (hora CDMX) |
| Snapshots | 5,501 (uno cada ~5 min; puede haber retrasos o huecos ocasionales) |
| Estaciones | 677 |

## Estructura

```
status/AAAA/MM/DD/HHMMSS.csv.gz      un snapshot de station_status; fecha y hora del nombre en UTC
info/AAAA-MM-DD_HHMMSS.json.gz       respuestas de station_information (catálogo), varias capturas
info/station_information_ultima.json.gz   la última captura del catálogo dentro del periodo
info/cambios.csv                     altas, bajas y cambios de capacidad detectados
SHA256SUMS                           hash de cada archivo
```

**Hora:** el nombre de cada archivo de `status/` está en **UTC**. La Ciudad de México está en UTC−6 todo el año (no tiene horario de verano desde 2022). Ejemplo: `status/2026/10/15/140000.csv.gz` corresponde al 15 de octubre a las 08:00 en la CDMX.

### Columnas de `status/*.csv.gz`

| Columna | Descripción |
|---|---|
| `station_id` | Identificador interno del GBFS. **No es el código de estación de los viajes**: crúzalo con `station_information` para obtener `short_name`. |
| `num_bikes_available` | Bicis disponibles para rentar |
| `num_bikes_disabled` | Bicis averiadas o bloqueadas |
| `num_docks_available` | Anclajes libres |
| `num_docks_disabled` | Anclajes fuera de servicio |
| `is_installed`, `is_renting`, `is_returning` | 1/0. Una estación con `is_renting = 0` está fuera de servicio |
| `last_reported` | Último reporte de la estación (epoch, segundos) |

### Catálogo (`info/*.json.gz`)

Formato GBFS `station_information`: `station_id`, `short_name` (el código de estación de los archivos de viajes, p. ej. `"041"` o `"266-267"`), `name`, `lat`, `lon`, `capacity`, entre otros campos.

## Cómo descargarlo

```bash
git clone https://github.com/GrupoGiganteDTI/ecobici-estado-estaciones.git
```

O descarga el ZIP desde el botón **Code → Download ZIP** de GitHub. Para verificar la integridad: `sha256sum -c SHA256SUMS`.

## Licencia

MIT, © 2026 Maximiliano Macías Herrera (autor de la captura original). Ver `LICENSE`.
