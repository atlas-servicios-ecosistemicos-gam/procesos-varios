# Publicación de geoservicios del Atlas de Servicios Ecosistémicos de la GAM

Este documento detalla el procedimiento para la publicación de los geoservicios (i.e. servicios web geoespaciales) del [Atlas de Servicios Ecosistémicos de la Gran Área Metropolitana](). Estos geoservicios son utilizados en la interfaz de usuario del Atlas y también están disponibles para ser consumidos con sistemas de información geográfica (SIG) como, por ejemplo, [ESRI ArcGIS](https://www.arcgis.com/) y [QGIS](https://qgis.org/). El documento incluye también una lista de los geoservicios publicados.

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
Los capas originales, en formato ESRI Shapefile, se transforman a formato GeoJSON, SRS WGS84 y con geometrías validadas. Estas transformaciones se realizan con la biblioteca [GDAL](https://gdal.org/), instalada en un ambiente [Conda](https://docs.conda.io/) (el anexo 1 describe el procedimiento para la creación y mantenimiento de este ambiente):
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
    -nln gam_parches_esenciales_importantes_bosque_corredores \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    gam_parches_esenciales_importantes_bosque_corredores.geojson \
    PARCHES_ESENCIALES_IMPORTANTES_BOSQUE.shp
    
# Parches esenciales e importantes para grupo funcional de bosque y bosque ribereño
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln gam_parches_esenciales_importantes_bosque_bripario_corredores \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    gam_parches_esenciales_importantes_bosque_bripario_corredores.geojson \
    PARCHES_ESENCIALES_IMPORTANTES_BOSQUE_BRIPARIO.shp
    
# Parches esenciales e importantes para grupo funcional bosque ribereño
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln gam_parches_esenciales_importantes_bripario_corredores \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    gam_parches_esenciales_importantes_bripario_corredores.geojson \
    PARCHES_ESENCIALES_IMPORTANTES_BRIPARIO.shp
    
# Parches esenciales e importantes para grupo funcional migratorias
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln gam_parches_esenciales_importantes_migratorias_corredores \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    gam_parches_esenciales_importantes_migratorias_corredores.geojson \
    PARCHES_ESENCIALES_IMPORTANTES_MIGRATORIAS.shp
    
# Parches esenciales e importantes para grupo funcional otras
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln gam_parches_esenciales_importantes_otras_corredores \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    gam_parches_esenciales_importantes_otras_corredores.geojson \
    PARCHES_ESENCIALES_IMPORTANTES_OTRAS.shp
```

Rutas de conectividad en CBI María Aguilar y Río Torres
```shell
# Rutas de conectividad para grupo funcional de bosque
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln gam_rutas_conectividad_bosque_corredores \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    gam_rutas_conectividad_bosque_corredores.geojson \
    RUTAS_CONECTIVIDAD_BOSQUE.shp
    
# Rutas de conectividad para grupo funcional de bosque y bosque ribereño ¡¡¡ESTE ARCHIVO ESTÁ GENERANDO UN ERROR AL CARGARSE EN AG ONLINE!!!
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln gam_rutas_conectividad_bosque_bripario_corredores \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    gam_rutas_conectividad_bosque_bripario_corredores.geojson \
    RUTAS_CONECTIVIDAD_BOSQUE_BRIPARIO.shp
    
# Rutas de conectividad para grupo funcional bosque ribereño
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln gam_rutas_conectividad_bripario_corredores \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    gam_rutas_conectividad_bripario_corredores.geojson \
    RUTAS_CONECTIVDAD_BRIPARIO.shp
    
# Rutas de conectividad para grupo funcional migratorias
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln gam_rutas_conectividad_migratorias_corredores \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    gam_rutas_conectividad_migratorias_corredores.geojson \
    RUTAS_CONECTIVIDAD_MIGRATORIAS.shp
    
# Rutas de conectividad para grupo funcional otras
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln gam_rutas_conectividad_otras_corredores \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    gam_rutas_conectividad_otras_corredores.geojson \
    RUTAS_CONECTIVIDAD_OTRAS.shp
```

##### GAM
```shell
$ cd ../GAM
```

Parches esenciales e importantes en GAM
```shell
# Parches esenciales e importantes para grupo funcional de bosque
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln gam_parches_esenciales_importantes_bosque_gam \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    gam_parches_esenciales_importantes_bosque_gam.geojson \
    PARCHES_ESENCIALES_IMPORTANTES_BOSQUE.shp
    
# Parches esenciales e importantes para grupo funcional de bosque y bosque ribereño
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln gam_parches_esenciales_importantes_bosque_bripario_gam \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    gam_parches_esenciales_importantes_bosque_bripario_gam.geojson \
    PARCHES_ESENCIALES_IMPORTANTES_BOSQUE_BRIPARIO.shp
    
# Parches esenciales e importantes para grupo funcional bosque ribereño
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln gam_parches_esenciales_importantes_bripario_gam \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    gam_parches_esenciales_importantes_bripario_gam.geojson \
    PARCHES_ESENCIALES_IMPORTANTES_BRIPARIO.shp
    
# Parches esenciales e importantes para grupo funcional migratorias
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln gam_parches_esenciales_importantes_migratorias_gam \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    gam_parches_esenciales_importantes_migratorias_gam.geojson \
    PARCHES_IMPORTANTES_ESENCIALES_MIGRATORIAS.shp
    
# Parches esenciales e importantes para grupo funcional otras
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln gam_parches_esenciales_importantes_otras_gam \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    gam_parches_esenciales_importantes_otras_gam.geojson \
    PARCHES_ESENCIALES_PRIORITARIOS_OTROS.shp
```

Rutas de conectividad en GAM
```shell
# Rutas de conectividad para grupo funcional de bosque
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln gam_rutas_conectividad_bosque_gam \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    gam_rutas_conectividad_bosque_gam.geojson \
    RUTAS_CONECTIVIDAD_BOSQUE.shp
    
# Rutas de conectividad para grupo funcional de bosque y bosque ribereño ¡¡¡ESTE ARCHIVO ESTÁ GENERANDO UN ERROR AL CARGARSE EN AG ONLINE!!!
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln gam_rutas_conectividad_bosque_bripario_gam \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    gam_rutas_conectividad_bosque_bripario_gam.geojson \
    RUTAS_CONECTIVIDAD_BOSQUE_BRIPARIO.shp
    
# Rutas de conectividad para grupo funcional bosque ribereño
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln gam_rutas_conectividad_bripario_gam \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    gam_rutas_conectividad_bripario_gam.geojson \
    RUTAS_CONECTIVIDAD_BRIPARIO.shp
    
# Rutas de conectividad para grupo funcional migratorias
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln gam_rutas_conectividad_migratorias_gam \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    gam_rutas_conectividad_migratorias_gam.geojson \
    RUTAS_CONECTIVIDAD_MIGRATORIAS.shp
    
# Rutas de conectividad para grupo funcional otras
$ ogr2ogr \
    -f "GeoJSON" \
    -progress \
    -nln gam_rutas_conectividad_otras_gam \
    -s_srs EPSG:5367 -t_srs EPSG:4326 \
    -makevalid \
    gam_rutas_conectividad_otras_gam.geojson \
    RUTAS_CONECTIVIDAD_OTRAS.shp
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
Los geoservicios se alojan en [GeoCatie](https://geocatie.maps.arcgis.com/) (plataforma ArcGIS Online de Catie), en la carpeta ```Atlas de servicios ecosistémicos de la GAM```. 

1. Con la opción *Agregar elemento*, debe cargarse cada uno de los archivos GeoJSON y marcarse la casilla *Publicar este archivo como una capa alojada*. La dirección del geoservicio correspondiente a la capa puede verse en el campo URL, en la pestaña "Información general" (ej. https://services9.arcgis.com/RrvMEynxDB8hycVO/arcgis/rest/services/parches_esenciales_importantes_bosque_corredores/FeatureServer).
2. El nivel de uso compartido de cada servicio debe especificarse como "Público".

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

**Parches esenciales e importantes en CBI María Aguilar y Río Torres**
<table>
  <thead>
    <tr><th>Id</th><th>Nombre de la capa</th><th>ArcGIS REST Feature Service</th><th>Visualización</th><th>Archivo original</th></tr>
  </thead>
  <tbody>
    <tr>
      <td>
        01
      </td>
      <td>
        Parches esenciales para grupo funcional de bosque en CBI María Aguilar y Río Torres
      </td>
      <td>
        https://services9.arcgis.com/RrvMEynxDB8hycVO/arcgis/rest/services/gam_parches_esenciales_importantes_bosque_corredores/FeatureServer/0
      </td>
      <td>
        https://atlas-servicios-ecosistemicos-gam.github.io/parches-esenciales-importantes-corredores/
      </td>
      <td>
        PARCHES_ESENCIALES_IMPORTANTES_BOSQUE.SHP
      </td>      
    </tr>
    <tr>
      <td>
        02
      </td>      
      <td>
        Parches importantes para grupo funcional de bosque en CBI María Aguilar y Río Torres
      </td>
      <td>
        https://services9.arcgis.com/RrvMEynxDB8hycVO/arcgis/rest/services/gam_parches_esenciales_importantes_bosque_corredores/FeatureServer/0
      </td>
      <td>
        https://atlas-servicios-ecosistemicos-gam.github.io/parches-esenciales-importantes-corredores/
      </td>
      <td>
        PARCHES_ESENCIALES_IMPORTANTES_BOSQUE.shp
      </td>      
    </tr>
    <tr>
      <td>
        03
      </td>      
      <td>
        Parches esenciales para grupo funcional de bosque y bosque ribereño en CBI María Aguilar y Río Torres
      </td>
      <td>
        https://services9.arcgis.com/RrvMEynxDB8hycVO/arcgis/rest/services/gam_parches_esenciales_importantes_bosque_bripario_corredores/FeatureServer/0
      </td>
      <td>
        https://atlas-servicios-ecosistemicos-gam.github.io/parches-esenciales-importantes-corredores/
      </td>
      <td>
        PARCHES_ESENCIALES_IMPORTANTES_BOSQUE_BRIPARIO.shp
      </td>          
    </tr>    
    <tr>
      <td>
        04
      </td>      
      <td>
        Parches importantes para grupo funcional de bosque y bosque ribereño en CBI María Aguilar y Río Torres
      </td>
      <td>
        https://services9.arcgis.com/RrvMEynxDB8hycVO/arcgis/rest/services/gam_parches_esenciales_importantes_bosque_bripario_corredores/FeatureServer/0
      </td>
      <td>
        https://atlas-servicios-ecosistemicos-gam.github.io/parches-esenciales-importantes-corredores/
      </td>
      <td>
        PARCHES_ESENCIALES_IMPORTANTES_BOSQUE_BRIPARIO.shp
      </td>          
    </tr>
    <tr>
      <td>
        05
      </td>      
      <td>
        Parches esenciales para grupo funcional de bosque ribereño en CBI María Aguilar y Río Torres
      </td>
      <td>
        https://services9.arcgis.com/RrvMEynxDB8hycVO/arcgis/rest/services/gam_parches_esenciales_importantes_bripario_corredores/FeatureServer/0
      </td>
      <td>
        https://atlas-servicios-ecosistemicos-gam.github.io/parches-esenciales-importantes-corredores/
      </td>
      <td>
        PARCHES_ESENCIALES_IMPORTANTES_BRIPARIO.shp
      </td>          
    </tr>    
    <tr>
      <td>
        06
      </td>      
      <td>
        Parches importantes para grupo funcional de bosque ribereño en CBI María Aguilar y Río Torres
      </td>
      <td>
        https://services9.arcgis.com/RrvMEynxDB8hycVO/arcgis/rest/services/gam_parches_esenciales_importantes_bripario_corredores/FeatureServer/0
      </td>
      <td>
        https://atlas-servicios-ecosistemicos-gam.github.io/parches-esenciales-importantes-corredores/
      </td>
      <td>
        PARCHES_ESENCIALES_IMPORTANTES_BRIPARIO.shp
      </td>          
    </tr>         
    <tr>
      <td>
        07
      </td>      
      <td>
        Parches esenciales para grupo funcional de migratorias en CBI María Aguilar y Río Torres
      </td>
      <td>
        https://services9.arcgis.com/RrvMEynxDB8hycVO/arcgis/rest/services/gam_parches_esenciales_importantes_migratorias_corredores/FeatureServer/0
      </td>
      <td>
        https://atlas-servicios-ecosistemicos-gam.github.io/parches-esenciales-importantes-corredores/
      </td>
      <td>
        PARCHES_ESENCIALES_IMPORTANTES_MIGRATORIAS.shp
      </td>          
    </tr>    
    <tr>
      <td>
        08
      </td>      
      <td>
        Parches importantes para grupo funcional de migratorias en CBI María Aguilar y Río Torres
      </td>
      <td>
        https://services9.arcgis.com/RrvMEynxDB8hycVO/arcgis/rest/services/gam_parches_esenciales_importantes_migratorias_corredores/FeatureServer/0
      </td>
      <td>
        https://atlas-servicios-ecosistemicos-gam.github.io/parches-esenciales-importantes-corredores/
      </td>
      <td>
        PARCHES_ESENCIALES_IMPORTANTES_MIGRATORIAS.shp
      </td>          
    </tr>            
    <tr>
      <td>
        09
      </td>      
      <td>
        Parches esenciales para grupo funcional de otras en CBI María Aguilar y Río Torres
      </td>
      <td>
        https://services9.arcgis.com/RrvMEynxDB8hycVO/arcgis/rest/services/gam_parches_esenciales_importantes_otras_corredores/FeatureServer/0
      </td>
      <td>
        https://atlas-servicios-ecosistemicos-gam.github.io/parches-esenciales-importantes-corredores/
      </td>
      <td>
        PARCHES_ESENCIALES_IMPORTANTES_OTRAS.shp
      </td>          
    </tr>    
    <tr>
      <td>
        10
      </td>      
      <td>
        Parches importantes para grupo funcional de otras en CBI María Aguilar y Río Torres
      </td>
      <td>
        https://services9.arcgis.com/RrvMEynxDB8hycVO/arcgis/rest/services/gam_parches_esenciales_importantes_otras_corredores/FeatureServer/0
      </td>
      <td>
        https://atlas-servicios-ecosistemicos-gam.github.io/parches-esenciales-importantes-corredores/
      </td>
      <td>
        PARCHES_ESENCIALES_IMPORTANTES_OTRAS.shp
      </td>          
    </tr>               
  </tbody>
</table>

**Rutas de conectividad en CBI María Aguilar y Río Torres**
<table>
  <thead>
    <tr><th>Id</th><th>Nombre de la capa</th><th>ArcGIS REST Feature Service</th><th>Visualización</th><th>Archivo original</th></tr>
  </thead>
  <tbody>
    <tr>
      <td>
        11
      </td>
      <td>
        Rutas de conectividad para grupo funcional de bosque en CBI María Aguilar y Río Torres
      </td>
      <td>
        https://services9.arcgis.com/RrvMEynxDB8hycVO/arcgis/rest/services/gam_rutas_conectividad_bosque_corredores/FeatureServer
      </td>
      <td>
      </td>
      <td>
        RUTAS_CONECTIVIDAD_BOSQUE.shp
      </td>      
    </tr>
    <tr>
      <td>
        12
      </td>
      <td>
        Rutas de conectividad para grupo funcional de bosque y bosque ribereño en CBI María Aguilar y Río Torres
      </td>
      <td>
        ERROR
      </td>
      <td>
      </td>
      <td>
        RUTAS_CONECTIVIDAD_BOSQUE_BRIPARIO.shp
      </td>      
    </tr>      
    <tr>
      <td>
        13
      </td>
      <td>
        Rutas de conectividad para grupo funcional de bosque ribereño en CBI María Aguilar y Río Torres
      </td>
      <td>
        https://services9.arcgis.com/RrvMEynxDB8hycVO/arcgis/rest/services/gam_rutas_conectividad_bripario_corredores/FeatureServer
      </td>
      <td>
      </td>
      <td>
        RUTAS_CONECTIVDAD_BRIPARIO.shp
      </td>      
    </tr>       
    <tr>
      <td>
        14
      </td>
      <td>
        Rutas de conectividad para grupo funcional de migratorias en CBI María Aguilar y Río Torres
      </td>
      <td>
        https://services9.arcgis.com/RrvMEynxDB8hycVO/arcgis/rest/services/gam_rutas_conectividad_migratorias_corredores/FeatureServer
      </td>
      <td>
      </td>
      <td>
        RUTAS_CONECTIVIDAD_MIGRATORIAS.shp
      </td>      
    </tr>
    <tr>
      <td>
        15
      </td>
      <td>
        Rutas de conectividad para grupo funcional de otras en CBI María Aguilar y Río Torres
      </td>
      <td>
        https://services9.arcgis.com/RrvMEynxDB8hycVO/arcgis/rest/services/gam_rutas_conectividad_otras_corredores/FeatureServer
      </td>
      <td>
      </td>
      <td>
        RUTAS_CONECTIVIDAD_OTRAS.shp
      </td>      
    </tr>            
  </tbody>
</table>
