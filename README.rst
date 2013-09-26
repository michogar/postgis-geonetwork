Curso de PostGIS 2.0 como repositorio de datos espaciales
#########################################################
Objetivo
########
Con la idea de introducir al alumnado en las bases de datos geoespaciales se desarrolla el siguiente curso. Se prepara a los mismos al manejo de estas comenzando por las nociones más básicas de teoría de bases de datos y acabando por operaciones complejas de análisis espacial. Además se dedica parte del temario al manejo de herramientas SIG con las que poder consultar estas bases de datos.

Al finalizar el curso el alumno podrá:
	* Instalar y configurar su propia base de datos
	* Crear un modelo de datos
	* Importar y exportar datos
	* Realizar operaciones de análisis

Destinatarios
#############
Este curso está destinado a usuarios del bases de datos que tengan algún conocimiento sobre **Modelo E/R** y manejo básico del lenguaje de consulta **SQL**. Durante el curso se dedicará un numero de horas al repaso de estas materias, pero se recomienda que los alumnos tengan cierto conocimiento de ellas para una mejor comprensión del curso.

Metodología
###########
El curso se realizará en **cuatro jornadas de siete horas** cada una en el horario indicado por los organizadores. 
Durante cada jornada se dedicará una parte del tiempo al estudio de la teoría con actividades prácticas intercaladas entre ella. A primera hora antes de comenzar cada jornada, se realizará un pequeño repaso de las partes estudiadas en la jornada anterior y consulta de dudas.

Temario
#######

#. Introducción
#. Teoría de bases de datos **(4h)**
	#. Tablas, columnas, registros
	#. Modelado bases de datos
	#. Normalización
	#. Diagrama E-R
	#. Restricciones
#. Instalación y creación base de datos **(2h)**
	#. Instalación y configuración PostgreSQL
	#. Clientes: pgsql y pgadmin3
	#. Creación roles y usuarios
#. Conceptos básicos SQL **(3h)**
	#. Definición de datos
	#. Manejo de datos
	#. Consultas
	#. Manejo de varias tablas
#. PostGIS, características espaciales **(2h)**
	#. Instalación y configuración PostGIS
	#. Tablas spatial_ref, geometry_columns
	#. Indices espaciales
	#. Funciones espaciales
	#. Módulos
		#. Vectorial
		#. Raster
		#. Topológico
	#. Tipos de datos espaciales
	#. Tipos de geometrías
#. Simple Feature Model **(2h)**
	#. OGC y el Simple feature model
	#. WKT y WKB
	#. Definición de geometrías básicas
		#. Points
		#. Linestrings
		#. Polygons
#. Relaciones espaciales **(3h)**
	#. Operaciones y operadores espaciales
		#. ST_Intersects
		#. ST_Touches
		#. ST_Crosses
		#. ...
#. Indexación espacial **(2h)**
	#. Creación índices espaciales
	#. Utilización índices espaciales
#. Importación y exportación de datos **(3h)**
	#. Importación shapes mediante shp2pgsql
	#. GDAL/OGR
		#. ogrinfo
		#. ogr2ogr
	#. Importación datos OSM a PostGIS
		#. osm2pgsql
		#. osmosis
	#. Consulta mediante visores web y sig escritorio
#. Análisis espacial **(4h)**
	#. Operadores espaciales
	#. Superposición
	#. Proximidad
	#. Generalización
	#. Transformación y edición de coordenadas
#. PostGIS Raster **(3h)**
	#. Definición de tipo raster
	#. Importación de ficheros raster

Anexo
#####
El curso se realizará utilizando la herramienta **PostGIS en su versión 2.0**. PostGIS es actualmente la referencia dentro de los sistemas gestores de bases de datos. Se trata de una herramienta de software libre, proyecto integrado en la Fundación OSGeo y utilizado por infinidad de centros.
Se entregará para la realización del mismo un CD con una máquina virtual con el software y los datos necesarios ya instalados.

Materiales necesarios
#####################
#. Conexión a internet
#. Proyector
#. Equipos con lector de CD y permiso de arranque desde el mismo


GeoNetwork como catálogo de metadatos
#####################################

Objetivo
########
Con este curso se pretende iniciar al alumno en el manejo de **GeoNetwork** como catálogo de metadatos dentro de un nodo IDE. Actualmente GeoNetwork es ha posicionado como uno de los servidores de metadatos mas potente del ecosistema de software libre orientado a geomática. Con ya muchos años de desarrollo e instalado en grandes organizaciones como la FAO (precursora originaria del proyecto) se muestra como una herramienta versatil, potente y altamente configurable. También se pretende mostrar brevemente la teoría que acompaña el uso de los metadatos dentro de una organización, tanto los estándares de servicio como las normas en las que los metadatos se apoyan para su definición. 

Se trata de un seminario básico de iniciación tanto al software como a los estándares.

Destinatarios
#############
Debido a la corta duración del seminario es recomendable que los alumnos estén familiarizados con conceptos como metadato, normas ISO, estándares OGC que serán descritos de manera somera durante la impartición del seminario, pero que son básicos para entender el concepto del software así como su utilidad. Conceptos como XML, Tomcat, servicio Web son también recomendables que sean conocidos por el alumno.

Temario
#######

#. Teoría sobre los metadatos **(1h)**
	#. ¿Qué son los metadatos?
	#. Definición de los diferentes normas
		#. ISO 19117
		#. Dublin Core
		#. ...
#. Estandar CSW para el manejo de metadatos **(2h)**
	#. Descripción del estandar
#. GeoNetwork como catálogo de metadatos **(5h)**
	#. Instalación y configuración
	#. Arquitectura de GeoNetwork
	#. Creación y publicación de metadatos
	#. Consumo de metadatos de diferentes nodos
	#. Configuración del servidor CSW
	
Anexo
#####
Este curso se impartirá con la versión 2.10 de GeoNetwork. El curso se realizará en una única jornada de **8 horas**.

Materiales necesarios
#####################
#. Conexión a internet
#. Proyector
#. Equipos con lector de CD y permiso de arranque desde el mismo