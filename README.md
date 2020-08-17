# Procesos varios del Atlas de servicios ecosistémicos de la GAM

## Creación de un ambiente Conda
```terminal
# Actualización de Conda
$ conda update -n base -c defaults conda

# Creación del ambiente
$ conda create -n gam

# Activación
$ conda activate gam

# Instalación de paquetes
$ conda install -c conda-forge gdal

# Desactivación (para el final del proceso)
$ conda deactivate
```

## Conversión de datos
### Conectividad
#### Corredores
```terminal
$ ogr2ogr \
  PARCHES_ESENCIALES_IMPORTANTES_BOSQUE_BRIPARIO_CORREDORES.geojson \
  PARCHES_ESENCIALES_IMPORTANTES_BOSQUE_BRIPARIO.shp \
  -f "GeoJSON" \
  -s_srs EPSG:5367 -t_srs EPSG:4326
```
