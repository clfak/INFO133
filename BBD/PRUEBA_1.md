## INFO133 Base de Datos
# PRUEBA 1

### Ejercicio 1: Modelamiento (~45 minutos)

__Caso de estudio: Base de datos “Sophia2”__

Un equipo de investigación valdiviana desarrolló varios scripts que recopilan datos sobre los distintos medios de prensa de varios países del mundo.

Por cada medio de prensa se conoce su nombre, el url de su sitio web en el cual publica noticias, el país, el continente del medio, su idioma principal y una categoría que indica si se trata de medio nacional, regional o local. Cada medio posee uno o varios dueños identificados por un nombre. Los dueños pueden ser personas reales o personas jurídicas. En la base de datos se quiere guardar el periodo en la cual un dueño posee un medio.

Cada medio puede poseer distintos canales sociales donde publica regularmente enlaces hacia sus noticias. Un canal social está identificado por el id de la cuenta y el nombre de la red social (por ejemplo Facebook, Twitter, etc.). Cada canal social tiene una audiencia que corresponde al número de followers que sigue este canal. El equipo de investigación indica que esta audiencia puede cambiar en el tiempo y que le interesa conservar la información histórica.

Los medios de prensa publican noticias que se caracterizan por un título, un texto, una fecha de publicación y opcionalmente una imagen de ilustración. El equipo de investigación considera que una noticia proviene de uno solo medio. Las noticias son escritas por un periodista al máximo. Interesa guardar en la base de datos el nombre y el género del periodista.

Finalmente, el equipo de investigación utiliza técnicas de tratamiento automático del lenguaje para extraer los nombres de personas, o fuentes, citadas en las noticias. Una noticia puede mencionar una o varias fuentes, y una misma fuente puede ser citada en varias noticias. Se guarda en la base de datos, el nombre y el género de estas fuentes, como tambíen la frase que menciona una fuente en una noticia.

* Proponer un modelo Entidad-Relación del caso de estudio siguiente. Utilizarán símbolos utilizados en clase para describir las ___entidades, relaciones, atributos___ y ___cardinalidades___. No indicarán las claves primarias y foráneas en su modelo. Su modelo no podrá incluir atributos multivaluados.

__R:__ [Respuesta](Diagrama_prueba1_sophia.png)

* A partir del modelo Entidad-Relación obtenido, realizar las transformaciones relevantes para obtener un modelo Relacional.Su Modelo Relacional deberá indicar explícitamente las claves primarias y foráneas. No es necesario especificar el tipo de datos asociado a cada atributo.

> **Medios_de_prensa**(___url___, nombre, pais,continente,idioma,categoría)

> **Canales**(___id_canal___,red_social, #url_media)

> **Periodistas** (___nombre___,género)

> **Noticias**(___id_noticia___, título, texto, fecha, imagen, #url_media, #nombrePeriodista)

> **Fuentes**(___nombre___,género)

> **Referencia**(___#nombreFuente,#id_noricia___,frase)

> **Duenos** (___nombreDueño___, tipo)

> **Posee**(___nombreDueño,#url_medio,inicio___, fin)


### Ejercicio 2: Consultas SQL 

Estamos utilizando la Base de Datos ‘classicmodels’ que almacena información de una empresa que vende modelos de autos. La Base de Datos tiene el 
modelo relacional siguiente:

__Nota bene:__ En este modelo relacional, los atributos de tipo “clave primaría” están indicado con un símbolo amarillo, y las claves foráneas tiene un símbolo rojo.

* Mostrar el apellido y nombre de cada cliente (tabla “Customers”) que viven en Chile y tienen un límite de crédito superior a 100.

~~~~
SELECT c.contactFirstName, c.contactLastName
FROM customers c
WHERE c.country = "Chile" 
AND c.creditlimit > 100;
~~~~

* Mostrar el apellido y nombre de cada cliente (tabla “Customers”), la fecha y el monto de cada pago que realizaron (tabla “Payments”), mostrando los pagos realizados entre el año 2010 y 2020.
~~~~
SELECT c.contactFirstName AS nombre_cliente, c.contactLastName AS apellido_cliente,amount AS monto
FROM customers c
JOIN payments p ON c.customerNumber = p.customerNumber
WHERE YEAR(p.paymentDate)
BETWEEN 2010 AND 2020;
~~~~

* Misma consulta que la anterior, pero agregando el número y el nombre del empleado que ha vendido los productos por cada pago realizado entre 2010 y 2020.

~~~~
SELECT c.contactFirstName AS nombre_cliente, c.contactLastName AS apellido_cliente, 
e.FirstName AS nombre_empleado, e.LastName AS apellido_empelado,amount AS monto
FROM customers c
JOIN payments p ON c.customerNumber = p.customerNumber
JOIN employees e ON c.SalesRepEmployeeNumber= e.employeeNumber
WHERE YEAR(p.paymentDate) 
BETWEEN 2010 AND 2020;
~~~~

* Utilizando la tabla “Payments”, mostrar en qué año el monto total de los pagos fue el más alto. (se podrá utilizar la clausúla “order by” o “limit”).

~~~
SELECT YEAR(p.paymentDate) AS year,  sum(p.amount) AS montoTotal 
FROM payments p 
GROUP BY YEAR(p.paymentDate) 
ORDER BY montoTotal DESC 
LIMIT 1;
~~~

* Mostrar el monto de pagos realizados (tabla "Payments") por cada empleado en el periodo 2010-2020
~~~
SELECT e.FirstName AS nombre_empleado, e.LastName AS apellido_empelado,  sum(p.amount) AS montoTotal
FROM payments p
JOIN customers c ON c.customerNumber = p.customerNumber
JOIN employees e ON p.SalesRepEmployeeNumber= e.employeeNumber
WHERE YEAR(p.paymentDate) BETWEEN 2010 AND 2020
GROUP BY e.employeeNumber
ORDER BY montoTotal;
~~~

* Misma consulta anterior, pero mostrando solamente los empleados que están asociados a un total de pagos superior a los $100.000

~~~~
SELECT e.employeeNumber, e.FirstName AS nombre_empleado, e.LastName AS apellido_empelado,  sum(p.amount) AS montoTotal 
FROM payments p 
JOIN customers c ON c.customerNumber = p.customerNumber 
JOIN employees e ON c.SalesRepEmployeeNumber= e.employeeNumber 
WHERE YEAR(p.paymentDate) BETWEEN 2010 AND 2020 
GROUP BY e.employeeNumber 
HAVING montoTotal >= 100000 
ORDER BY montoTotal DESC;
~~~~

```
```









