# Publicación de geoservicios del Atlas de Servicios Ecosistémicos de la GAM

Este repositorio detalla el procedimiento para la publicación de los geoservicios (i.e. servicios web geoespaciales) utilizados en el Atlas de Servicios Ecosistémicos de la GAM. Estos geoservicios son utilizados para construir la interfaz de usuario del Atlas en []() y también están disponibles para acceso público en []().

La siguiente es la lista de capas publicadas a la fecha como geoservicios:

<table>
  <thead>
    <tr><th>Nombre de la capa</th><th>ArcGIS REST Feature Service</th><th>Visualización</th><th>Archivo original</th></tr>
  </thead>
  <tbody>
    <tr>
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

El procedimiento consiste de los siguientes pasos:

0. Clonación de este repositorio.
1. Obtención de las capas geoespaciales.
2. Transformación de las capas.
3. Publicación de geoservicios en ArcGIS Online.

### 0. Clonación de este repositorio
```shell
# Clonación de este repositorio
$ git clone https://github.com/atlas-servicios-ecosistemicos-gam/publicacion-geoservicios.git
$ cd publicacion-geoservicios
```

### 1. Obtención de las capas geoespaciales
Las capas geoespaciales que se publican en los geoservicios se descargan, en formato ZIP, de un servidor FTP del Catie:
```shell
$ ftp 165.227.80.21
ftp> cd PARA_ATLAS
ftp> hash
ftp> get CONECTIVIDAD_GAM.zip
ftp> get INFRAESTRUCTURA_VERDE_CORREDORES.zip
ftp> quit
$ unzip CONECTIVIDAD_GAM.zip
$ unzip INFRAESTRUCTURA_VERDE_CORREDORES.zip
```

### 2. Transformación de las capas
Los capas originales, en formato ESRI Shapefile, se transforman a formato GeoJSON, SRS WGS84 y con geometrías validadas. Estas transformaciones se realizan con la biblioteca GDAL, instalada en un ambiente Conda:
```shell
```

#### Conectividad
##### Corredores
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

### Publicación de geoservicios en ArcGIS Online

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
