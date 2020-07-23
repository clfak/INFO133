## INFO133 Base de Datos
# PRUEBA 1

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

















