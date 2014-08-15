---
layout: post
title: Manipulacion de XMLs directamente desde el MSSQL Server
date: 2009-1-29
tags: XML SQL MSSQL
comments: true
permalink:
share: true
---

Por si a alguien le interesa el tema de la manipulacion de XMLs directamente desde el servidor MSSQL Server

Ire publicando aqui varios ejemplos

1er Ejemplo : Consulta de un xml y uso de la sentencia WHERE

DECLARE @LISTA1 VARCHAR(MAX)

SET @LISTA1 =

N'

'

* Arriba creamos un XML de ejemplo con elemento principal LISTA1 y varios ELEMENTOs y procederemos a declarar el Objeto que amlacenara este TEXTO y para ello usaremos el objeto o tipo de dato XML que viene en el SQLServer

DECLARE @XMLLIST1 XML

* Le asignamos el contenido de la variable @LISTA1 al tipo XML previamente creado , objeto que sera el que nos permita manipular el texto de una mejor manera

SET @XMLLIST1 = @LISTA1

* A continuacion hacemos un SELECT siemple que nos devuelve todos los ELEMENTOs que hay en el XML, notar que en la sentencia de abajo hay dos barras delante de ELEMENTO, esto indica que queremos realizar la consulta de todos los //ELEMENTO que existen sin importar el nivel en el que se encuentre

SELECT Elemento = T.Item.value('@VALOR', 'varchar(25)')

FROM @XMLLIST1.nodes('//ELEMENTO') AS T(item)

* Ahora supongamos que solo queremos mostrar el ELEMENTO con valor 1 , para ello tendriamos que añadir la siguiente linea

WHERE T.Item.value('@VALOR', 'varchar(25)') = 1

* Donde el signo **@ **indica que nos referimos a un atributo dentro de ELEMENTO, como se puede apreciar tuvimos que volver a declarar el @VALOR , y si tuvieramos 5 atributos que filtrar deberia hacer esto por cada uno de los cinco , por eso recomiendo hacer lo siguiente para que asi sea mas facil la manipulacion del xml

SELECT *

FROM (

SELECT Elemento = T.Item.value('@VALOR', 'varchar(25)')

FROM @XMLLIST1.nodes('//ELEMENTO') AS T(item)

)LISTA1

WHERE ELEMENTO = 1

* Asi de esta manera los campos que se hayan declarado en el interior del FROM pues sera mucho mas facil usarlos desde fuera ya que solo los llamaremos por su nombre y no asi por la declaracion XML : T.Item.value('@VALOR', 'varchar(25)')

 

"));