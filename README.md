# Procesos varios del Atlas de servicios ecosistémicos de la GAM

## Creación y mantenimiento de un ambiente Conda
**Creación**
```terminal
# Actualización de Conda
$ conda update -n base -c defaults conda

# Creación del ambiente
$ conda create -n gam
```

**Activación**
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
**Desactivación**
```terminal
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
