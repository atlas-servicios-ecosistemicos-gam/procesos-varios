# Procesos varios del Atlas de servicios ecosistémicos de la GAM

## Creación y mantenimiento de un ambiente Conda
**Actualización de Conda**
```terminal
# Actualización de Conda
$ conda update -n base -c defaults conda
```

**Creación del ambiente**
```terminal
# Creación del ambiente
$ conda create -n gam
```

**Activación del ambiente**
```terminal
# Activación del ambiente
$ conda activate gam
```

**Instalación de paquetes**
```terminal
# Instalación de paquetes
$ conda install -c conda-forge gdal
$ conda install -c conda-forge qgis
```
**Desactivación del ambiente**
```terminal
# Desactivación (para el final del proceso)
$ conda deactivate
```

## Conversión de datos
### Conectividad
#### Corredores
```bash
$ ogr2ogr \
  parches_esenciales_importantes_bosque_corredores.geojson \
  PARCHES_ESENCIALES_IMPORTANTES_BOSQUE.shp \
  -f "GeoJSON" \
  -progress \
  -nln parches_esenciales_importantes_bosque_corredores \
  -s_srs EPSG:5367 -t_srs EPSG:4326 \
  -makevalid
```
