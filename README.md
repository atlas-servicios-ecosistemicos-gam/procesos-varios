# Publicación de geoservicios del Atlas de Servicios Ecosistémicos de la GAM

## Descripción general
Este repositorio detalla el procedimiento para la publicación de los geoservicios (i.e. servicios web geoespaciales) utilizados en el Atlas de Servicios Ecosistémicos de la GAM. Estos geoservicios son utilizados para construir la interfaz de usuario del Atlas en []() y también están disponibles para acceso público en []().

El procedimiento consiste de los siguientes pasos:

0. Clonación del repositorio y activación del ambiente Conda.
1. Obtención de las capas geoespaciales.
2. 

### Clonación del repositorio y activación del ambiente Conda
```shell
# Activación del ambiente Conda
$ conda activate gam

# Clonación del repositorio
$ git clone https://github.com/atlas-servicios-ecosistemicos-gam/publicacion-geoservicios.git
$ cd publicacion-geoservicios
```

### Obtención de las capas geoespaciales
Las capas geoespaciales que se publican en los geoservicios fueron descargadas de un servidor FTP del Catie:
```shell
$ ftp 165.227.80.21
ftp> cd PARA_ATLAS
ftp> hash
ftp> get CONECTIVIDAD_GAM.zip
ftp> get INFRAESTRUCTURA_VERDE_CORREDORES.zip
ftp> quit
```

```shell
$ unzip CONECTIVIDAD_GAM.zip
$ unzip INFRAESTRUCTURA_VERDE_CORREDORES.zip
```

### Desactivación del ambiente Conda
```shell
# Desactivación del ambiente Conda
$ conda deactivate
```

## Creación y mantenimiento de un ambiente Conda
**Actualización de Conda**
```shell
# Actualización de Conda
$ conda update -n base -c defaults conda
```

**Creación del ambiente**
```shell
# Creación del ambiente
$ conda create -n gam
```

**Activación del ambiente**
```shell
# Activación del ambiente
$ conda activate gam
```

**Instalación de paquetes**
```shell
# Instalación de paquetes
$ conda install -c conda-forge gdal
$ conda install -c conda-forge qgis
```
**Desactivación del ambiente**
```shell
# Desactivación (para el final del proceso)
$ conda deactivate
```

## Conversión de datos
### Conectividad
#### Corredores
```shell
# Parches esenciales e importantes para grupo funcional de bosque
$ ogr2ogr \
  parches_esenciales_importantes_bosque_corredores.geojson \
  PARCHES_ESENCIALES_IMPORTANTES_BOSQUE.shp \
  -f "GeoJSON" \
  -progress \
  -nln parches_esenciales_importantes_bosque_corredores \
  -s_srs EPSG:5367 -t_srs EPSG:4326 \
  -makevalid
```
