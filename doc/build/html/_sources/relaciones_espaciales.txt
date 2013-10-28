
*********************
Relaciones espaciales
*********************

Introducción
============

Estos métodos lo que hacen es verificar el cumplimiento de determinados predicados geográficos entre dos geometrías distintas. Los predicados geográficos toman dos geometrías como argumento, y devuelven un valor booleano que indica si ambas geometrías cumplen o no una determinada relación espacial. Las principales relaciones espaciales contempladas son equals, disjoint, intersects, touches, crosses, within, contains, overlaps. 

Matriz DE-9IM
=============

Estas relaciones o predicados son descritas por la matriz DE-9IM (Dimensionally Extended 9 intersection Matrix), que es una construcción matemática de la rama de la topología.

	.. image:: _images/matriz.png
		:scale: 50 %

Figura: Mátriz DE-9IM de dos figuras geométricas dadas. Fuente: wikipedia en inglés [7] (http://en.wikipedia.org/wiki/DE-9IM)

Predicados espaciales
=====================

	.. image:: _images/mas_predicados_espaciales.png
		:scale: 50 %
		
Figura: Ejemplos de predicados espaciales. Fuente: wikipedia. http://en.wikipedia.org/wiki/File:TopologicSpatialRelarions2.png

	.. image:: _images/touches.png
		:scale: 50 %

Figura: Ejemplos de la relación “Touch” (toca). Fuente: “OpenGIS® Implementation Standard for Geographic information - Simple feature access - Part 1: Common architecture”

	.. image:: _images/crosses.png
		:scale: 50 %

Figura: Ejemplos de la relación “Crosses” (cruza). Fuente: “OpenGIS® Implementation Standard for Geographic information - Simple feature access - Part 1: Common architecture”

	.. image:: _images/within.png	
		:scale: 50 %
	
Figura: Ejemplos de la relación “Within” (contenido en). Fuente: “OpenGIS® Implementation Standard for Geographic information - Simple feature access - Part 1: Common architecture”

	.. image:: _images/overlaps.png
		:scale: 50 %

Figura: Ejemplos de la relación “Overlaps” (solapa). Fuente: “OpenGIS® Implementation Standard for Geographic information - Simple feature access - Part 1: Common architecture”

Los principales métodos de la clase Geometry para chequear predicados espaciales entra la geometría en cuestión y otra proporcionada como parámetro son:

	* **Equals (A, B)**: Las geometrías son iguales desde un punto de vista topológico
	* **Disjoint (A, B)**: No tienen ningún punto en común, las geometrías son disjuntas
	* **Intersects (A, B)**:Tienen por lo menos un punto en común. Es el inverso de Disjoint
	* **Touches (A, B)**: Las geometrías tendrán por lo menos un punto en común del contorno, pero no puntos interiores
	* **Crosses (A, B)**: Comparten parte, pero no todos los puntos interiores, y la dimensión de la intersección es menor que la dimensión de al menos una de las geometrías
	* **Contains (A, B)**: Ningún punto de B está en el exterior de A. Al menos un punto del interior de B está en el interior de A
	* **Within (A, B)**: Es el inverso de Contains. Within(B, A) = Contains (A, B)
	* **Overlaps (A, B)**: Las geometrías comparten parte pero no todos los puntos y la intersección tiene la misma dimensión que las geometrías.
	* **Covers (A, B)**: Ningún punto de B está en el exterior de A. B está contenido en A.
	* **CoveredBy (A, B)**: Es el inverso de Covers. CoveredBy(A, B) = Covers(B, A)

ST_Equals
---------
::

	ST_Equals(geometry A, geometry B), comprueba que dos geometrías sean espacialmente iguales.

ST_Equals devuelve TRUE si dos geometrías del mismo tipo tienen identicas coordenadas x,y. 

Ejemplo
^^^^^^^
::
	
	# SELECT name, the_geom, ST_AsText(the_geom)
	FROM nyc_subway_stations
	WHERE name = 'Broad St';

::

	   name   |                      the_geom                      |      st_astext
	----------+----------------------------------------------------+-----------------------
	 Broad St | 0101000020266900000EEBD4CF27CF2141BC17D69516315141 | POINT(583571 4506714)	

Si usamos el valor obtenido en ``the_geom`` y consultamos a la base de datos::

	# SELECT name
	FROM nyc_subway_stations
	WHERE ST_Equals(the_geom, '0101000020266900000EEBD4CF27CF2141BC17D69516315141');

::

	Broad St

ST_Intersects, ST_Disjoint, ST_Crosses y ST_Overlaps
----------------------------------------------------
Comprueban la relación entre los interiores de las geometrías.

ST_Intersects
^^^^^^^^^^^^^
::

	ST_Intersects(geometry A, geometry B) 
	
Devuelve TRUE si la intersección no es un resultado vacio. 

ST_Disjoint
^^^^^^^^^^^
::

	ST_Disjoint(geometry A , geometry B)
	
Es el inverso de ST_Intersects. indica que dos geometrías no tienen ningún punto en común. Es menos eficiente que ST_Intersects ya que esta no está indexada. Se recomienda comprobar ``NOT ST_Intersects``

ST_Crosses
^^^^^^^^^^
::

	ST_Crosses(geometry A, geometry B)
	
Se cumple esta relación si el resultado de la intesección de dos geometrías es de dimensión menor que la mayor de las dimensiones de las dos geometrías y además esta intersección está en el interior de ambas.

ST_Overlap
^^^^^^^^^^
::

	ST_Overlaps(geometry A, geometry B) 
	
compara dos geometrías de la misma dimensión y devuelve TRUE si su intersección resulta una geometría diferente de ambas pero de la misma dimensión

Ejemplo
"""""""

Compruebe el barrio donde se encuentra la estación de metro *Broad Street*::

	# SELECT name, ST_AsText(the_geom)
	FROM nyc_subway_stations
	WHERE name = 'Broad St';
	
::

	POINT(583571 4506714)
	
::

	# SELECT name, boroname
	FROM nyc_neighborhoods
	WHERE ST_Intersects(the_geom, ST_GeomFromText('POINT(583571 4506714)',26918));
	
::

		     name        | boroname
	--------------------+-----------
	 Financial District | Manhattan
	

ST_Touches
^^^^^^^^^^
::

	ST_Touches(geometry A, geometry B)
	
Devuelte TRUE si cualquiera de los contornos de las geometrías se cruzan o si sólo uno de los interiores de la geometría se cruza el contorno del otro.
	
ST_Within y ST_Contains
^^^^^^^^^^^^^^^^^^^^^^^
::

	ST_Within(geometry A , geometry B)
	
es TRUE si la geometría A está completamente dentro de la geometría B. Es el inverso de ST_Contains

::

	ST_Contains(geometry A, geometry B)
	
Devuelve TRUE si la geometría B está contenida completamente en la geometría A

Ejemplo
"""""""

¿En que barrio se encuentra la estación Brook Ave?

::

	# SELECT ST_AsText(the_geom) 
	FROM nyc_subway_stations 
	WHERE name='Brook Ave';
	
::

	POINT(591158.462734484 4517957.5457551)
	
::

	# SELECT boroname 
	FROM nyc_neighborhoods as n 
	WHERE ST_Contains(n.the_geom, ST_GeometryFromText('POINT(591158.462734484 4517957.5457551)',26918));
	
:: 

	 boroname  
	-----------
	 The Bronx

ST_Distance and ST_DWithin
^^^^^^^^^^^^^^^^^^^^^^^^^^
::

	ST_Distance(geometry A, geometry B)
	
Calcula la menor distancia entre dos geometrías.

::

	ST_DWithin(geometry A, geometry B, distance)
	
Permite calcular si dos objetos se encuentran a una distancia dada uno del otro.

Ejemplo
"""""""
Encontrar las calles que estén a menos de 10 metros de la estación de metro Broad Street

::
	
	# SELECT name
	FROM nyc_streets
	WHERE ST_DWithin(
		     the_geom,
		     ST_GeomFromText('POINT(583571 4506714)',26918),
		     10
		   );
	
::

	     name
	--------------
		Wall St
		Broad St
		Nassau St

JOINS espaciales
================
Permite combinar información de diferentes tablas usando relaciones espaciales como clvae dentro del JOIN. En los ejemplos anteriores hemos realizado el proceso en varios pasos. Usando JOINS espaciales, podremos realizarlos en un solo paso::

	# SELECT
	  subways.name AS subway_name,
	  neighborhoods.name AS neighborhood_name,
	  neighborhoods.boroname AS borough
	FROM nyc_neighborhoods AS neighborhoods
	JOIN nyc_subway_stations AS subways
	ON ST_Contains(neighborhoods.the_geom, subways.the_geom)
	WHERE subways.name = 'Broad St';
	
::

	 subway_name | neighborhood_name  |  borough
	-------------+--------------------+-----------
	 Broad St    | Financial District | Manhattan	
	 
Cualquier función que permita crear relaciones TRUE/FALSE entre dos tablas puede ser usada para manejar un JOIN espacial, pero comunmente las más usadas son:

	* ST_Intersects
	* ST_Contains
	* ST_DWithin
	
JOIN y GROUP BY
---------------
Para la pregunta ¿Cúal es la población de los barrios de Manhattan agrupada por color de piel?, habremos de combinar la información del censo con los límites de los barrios con la restricción de que sea para el barrio de Manhattan::

	# SELECT
	  neighborhoods.name AS neighborhood_name,
	  Sum(census.popn_total) AS population,
	  Round(100.0 * Sum(census.popn_white) / Sum(census.popn_total),1) AS white_pct,
	  Round(100.0 * Sum(census.popn_black) / Sum(census.popn_total),1) AS black_pct
	FROM nyc_neighborhoods AS neighborhoods
	JOIN nyc_census_blocks AS census
	ON ST_Intersects(neighborhoods.the_geom, census.the_geom)
	WHERE neighborhoods.boroname = 'Manhattan'
	GROUP BY neighborhoods.name
	ORDER BY white_pct DESC;

::

	  neighborhood_name  | population | white_pct | black_pct
	---------------------+------------+-----------+-----------
	 Carnegie Hill       |      19909 |      91.6 |       1.5
	 North Sutton Area   |      21413 |      90.3 |       1.2
	 West Village        |      27141 |      88.1 |       2.7
	 Upper East Side     |     201301 |      87.8 |       2.5
	 Greenwich Village   |      57047 |      84.1 |       3.3
	 Soho                |      15371 |      84.1 |       3.3
	 Murray Hill         |      27669 |      79.2 |       2.3
	 Gramercy            |      97264 |      77.8 |       5.6
	 Central Park        |      49284 |      77.8 |      10.4
	 Tribeca             |      13601 |      77.2 |       5.5
	 Midtown             |      70412 |      75.9 |       5.1
	 Chelsea             |      51773 |      74.7 |       7.4
	 Battery Park        |       9928 |      74.1 |       4.9
	 Upper West Side     |     212499 |      73.3 |      10.4
	 Financial District  |      17279 |      71.3 |       5.3
	 Clinton             |      26347 |      64.6 |      10.3
	 East Village        |      77448 |      61.4 |       9.7
	 Garment District    |       6900 |      51.1 |       8.6
	 Morningside Heights |      41499 |      50.2 |      24.8
	 Little Italy        |      14178 |      39.4 |       1.2
	 Yorkville           |      57800 |      31.2 |      33.3
	 Inwood              |      50922 |      29.3 |      14.9
	 Lower East Side     |     104690 |      28.3 |       9.0
	 Washington Heights  |     187198 |      26.9 |      16.3
	 East Harlem         |      62279 |      20.2 |      46.2
	 Hamilton Heights    |      71133 |      14.6 |      41.1
	 Chinatown           |      18195 |      10.3 |       4.2
	 Harlem              |     125501 |       5.7 |      80.5	
	 
1. La clausula JOIN crea una tabla virtual que incluye los datos de los barrios y de los censos
2. WHERE filtra la tabla virtual solo para las columnas de Manhattan
3. Las filas resultantes son agrupadas por el nombre del barrio y rellenadas con la función de agregación Sum() de los valores de población

Práctica
========

1. Calcula la matriz DE-9IM entre AB y BA para las siguientes figuras:

	.. image:: _images/relations-exec/geoms.png
		:scale: 50 %
	
	.. image:: _images/relations-exec/geoms2.png
		:scale: 50 %
		
	.. image:: _images/relations-exec/geoms3.png
		:scale: 50 %
			
	.. image:: _images/relations-exec/geoms4.png
		:scale: 50 %
		
2. Dibuja dos casos de geometrías con las que se obtengan las siguientes matrices:

	* 0F0FFF102
	* FF2F112F2
	
3. Indica la respuesta correcta en los siguientes casos:

	* **a** ¿En que casos se aplica el predicado Touches?
	
		.. image::  _images/relations-exec/touches1.png
			:scale: 50 %
		
		.. image::  _images/relations-exec/touches2.png	
			:scale: 50 %	
			
	* **b** ¿Y el predicado Crosses?
	
		.. image::  _images/relations-exec/crosses1.png
			:scale: 50 %
		
		.. image::  _images/relations-exec/crosses2.png	
			:scale: 50 %	
			
	* **c** ¿Comprueba en que casos se aplica el predicado Overlaps?
	
		.. image::  _images/relations-exec/overlaps1.png
			:scale: 50 %
		
		.. image::  _images/relations-exec/overlaps2.png	
			:scale: 50 %
	
4. Utilizando el software JTS Test Builder, generar un ejemplo que cumpla cada una de las relaciones.

5. Comprueba si estas geometrías son iguales: LINESTRING(0 0, 10 0) Y MULTILINESTRING((10 0, 5 0),(0 0, 5 0)).

6. Represente como texto el valor de la geometría de la calle 'Atlantic Commons'.

7. ¿En que barrio se encuentra Atlantic Commons?

8. ¿Qué calles colindan con Atlantic Commons?

9. Aproximadamente, ¿cuánta gente vive en los 50 m alrededor de Atlantic Commons?

10. ¿Cúal es la calle más larga de NY y que barrios cruza?

11. ¿Qué estación de metro está en Little Italy? ¿Qué linea de metro es?

12. ¿Cuales son los barrios por los que pasa la linea 6?. Recuerda que las lineas (routes) son de tipo texto y pueden estar compuestas por una o varias lineas (routes='B,D,6,V'). Utiliza la función ``strpos``

13. Después del 11-S, el barrio Battery Park estuvo cerrado por varios dias. ¿Cuanta gente tuvo que ser evacuada?
