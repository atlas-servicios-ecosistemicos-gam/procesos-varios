# Publicación de geoservicios del Atlas de Servicios Ecosistémicos de la GAM

Este documento detalla el procedimiento para la publicación de los geoservicios (i.e. servicios web geoespaciales) del [Atlas de Servicios Ecosistémicos de la Gran Área Metropolitana](). Estos geoservicios son utilizados en la interfaz de usuario del Atlas y también están disponibles para ser consumidos con sistemas de información geográfica, como [ESRI ArcGIS](https://www.arcgis.com/) y [QGIS](https://qgis.org/), entre otras. Se incluye también una lista de los geoservicios publicados.

## Contenido
- [1. Procedimiento para la publicación de geoservicios](#1-procedimiento-para-la-publicaci%C3%B3n-de-geoservicios)
    - [1.0. Clonación de este repositorio Git](#10-clonaci%C3%B3n-de-este-repositorio-git)
    - [1.1. Obtención de las capas geoespaciales](#11-obtenci%C3%B3n-de-las-capas-geoespaciales)
    - [1.2. Transformación de las capas geoespaciales](#12-transformaci%C3%B3n-de-las-capas-geoespaciales)
    - [1.3. Publicación de geoservicios en ArcGIS Online](#13-publicaci%C3%B3n-de-geoservicios-en-arcgis-online)
- [Anexo 1. Procedimiento para la creación y mantenimiento de un ambiente Conda](#anexo-1-procedimiento-para-la-creaci%C3%B3n-y-mantenimiento-de-un-ambiente-conda)
- [Anexo 2. Lista de geoservicios publicados](#anexo-2-lista-de-geoservicios-publicados)

## 1. Procedimiento para la publicación de geoservicios
El procedimiento de publicación de los geoservicios consiste de los siguientes pasos:

- [1.0. Clonación de este repositorio Git](#10-clonaci%C3%B3n-de-este-repositorio-git)
- [1.1. Obtención de las capas geoespaciales](#11-obtenci%C3%B3n-de-las-capas-geoespaciales)
- [1.2. Transformación de las capas geoespaciales](#12-transformaci%C3%B3n-de-las-capas-geoespaciales)
- [1.3. Publicación de geoservicios en ArcGIS Online](#13-publicaci%C3%B3n-de-geoservicios-en-arcgis-online)

### 1.0. Clonación de este repositorio Git
```shell
# Clonación de este repositorio Git
$ git clone https://github.com/atlas-servicios-ecosistemicos-gam/publicacion-geoservicios.git

$ cd publicacion-geoservicios
```

### 1.1. Obtención de las capas geoespaciales
Las capas geoespaciales que se publican en los geoservicios se descargan, en formato ZIP, de un servidor FTP del Catie:
```shell
# Descarga de los archivos
$ ftp 165.227.80.21 # deben ingresarse el usuario y la clave
ftp> cd PARA_ATLAS
ftp> hash
ftp> get CONECTIVIDAD_GAM.zip
ftp> get INFRAESTRUCTURA_VERDE_CORREDORES.zip
ftp> quit

# Descompresión
$ unzip CONECTIVIDAD_GAM.zip
$ unzip INFRAESTRUCTURA_VERDE_CORREDORES.zip
```
Una vez descomprimidos, se recomienda guardar los archivos ZIP fuera del repositorio o no incluirlos en la operación ```git add```, ya que son demasiado grandes para ser aceptados por GitHub.

### 1.2. Transformación de las capas geoespaciales
Los capas originales, en formato ESRI Shapefile, se transforman a formato GeoJSON, SRS WGS84 y con geometrías validadas. Estas transformaciones se realizan con la biblioteca [GDAL](https://gdal.org/), instalada en un ambiente [Conda](https://docs.conda.io/):
```shell
# Activación del ambiente Conda
$ conda activate gam
```

Posteriormente, se procede a generar los archivos GeoJSON correspondientes a los shapefiles, los cuales están ubicados en diferentes subdirectorios:
#### Conectividad
```shell
$ cd CONECTIVIDAD_GAM
```
##### Corredores
```shell
$ cd CORREDORES
```

Parches esenciales e importantes en CBI María Aguilar y Río Torres
```shell
# Parches esenciales e importantes para grupo funcional de bosque
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln parches_esenciales_importantes_bosque_corredores \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    parches_esenciales_importantes_bosque_corredores.geojson \
    PARCHES_ESENCIALES_IMPORTANTES_BOSQUE.shp
    
# Parches esenciales e importantes para grupo funcional de bosque y bosque ribereño
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln parches_esenciales_importantes_bosque_bripario_corredores \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    parches_esenciales_importantes_bosque_bripario_corredores.geojson \
    PARCHES_ESENCIALES_IMPORTANTES_BOSQUE_BRIPARIO_CORREDORES.SHP
    
# Parches esenciales e importantes para grupo funcional bosque ribereño
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln parches_esenciales_importantes_bripario_corredores \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    parches_esenciales_importantes_bripario_corredores.geojson \
    PARCHES_ESENCIALES_IMPORTANTES_BRIPARIO_CORREDORES.SHP
    
# Parches esenciales e importantes para grupo funcional migratorias
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln parches_esenciales_importantes_migratorias_corredores \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    parches_esenciales_importantes_migratorias_corredores.geojson \
    PARCHES_ESENCIALES_IMPORTANTES_MIGRATORIAS_CORREDORES.SHP
    
# Parches esenciales e importantes para grupo funcional otras
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln parches_esenciales_importantes_otras_corredores \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    parches_esenciales_importantes_otras_corredores.geojson \
    PARCHES_ESENCIALES_IMPORTANTES_OTRAS_CORREDORES.SHP    
```

Una vez finalizadas las transformaciones, este repositorio Git debe actualizarse y el ambiente Conda debe desactivarse:
```shell
# Actualización del repositorio
$ git status
$ git add .
$ git commit -m "Transformar datos"
$ git push

# Desactivación del ambiente Conda
$ conda deactivate
```

### 1.3. Publicación de geoservicios en ArcGIS Online
Los geoservicios se alojan en [GeoCatie](https://geocatie.maps.arcgis.com/) (plataforma ArcGIS Online de Catie), en la carpeta ```Atlas de servicios ecosistémicos de la GAM```. Con la opción *Agregar elemento*, debe cargarse cada uno de los archivos GeoJSON y marcarse la casilla *Publicar este archivo como una capa alojada*.

## Anexo 1. Procedimiento para la creación y mantenimiento de un ambiente Conda
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

## Anexo 2. Lista de geoservicios publicados
La siguiente es la lista de geoservicios publicados a la fecha:

<table>
  <thead>
    <tr><th>Id</th><th>Nombre de la capa</th><th>ArcGIS REST Feature Service</th><th>Visualización</th><th>Archivo original</th></tr>
  </thead>
  <tbody>
    <tr>
      <td>
        1
      </td>
      <td>
        Parches esenciales para grupo funcional de bosque en CBI María Aguilar y Río Torres
      </td>
      <td>
        https://services9.arcgis.com/RrvMEynxDB8hycVO/arcgis/rest/services/parches_esenciales_importantes_bosque_corredores/FeatureServer/0
      </td>
      <td>
        https://atlas-servicios-ecosistemicos-gam.github.io/parches-esenciales-importantes-bosque-corredores/
      </td>
      <td>
        PARCHES_ESENCIALES_IMPORTANTES_BOSQUE_CORREDORES.SHP
      </td>      
    </tr>
    <tr>
      <td>
        2
      </td>      
      <td>
        Parches importantes para grupo funcional de bosque en CBI María Aguilar y Río Torres
      </td>
      <td>
        https://services9.arcgis.com/RrvMEynxDB8hycVO/arcgis/rest/services/parches_esenciales_importantes_bosque_corredores/FeatureServer/0
      </td>
      <td>
        https://atlas-servicios-ecosistemicos-gam.github.io/parches-esenciales-importantes-bosque-corredores/
      </td>
      <td>
        PARCHES_ESENCIALES_IMPORTANTES_BOSQUE_CORREDORES.SHP
      </td>      
    </tr>    
  </tbody>
</table>
