## INFO133: Consultas para la base de datos Classicmodels

Diferencias entre __SELECT__ y __DISTINCT__:

* Mostrar en qué ciudad viven los clientes 

~~~~
SELECT city 
FROM customers 
ORDER BY city ASC;
~~~~

* Misma pregunta pero evitando mostrar varias veces la misma ciudad: 

~~~~
SELECT DISTINCT city 
FROM customers 
ORDER BY city ASC;
~~~~

__ORDER BY__

~~~~
ELECT contactLastname, contactFirstname 
FROM customers 
ORDER BY contactLastname ASC, contactFirstname ASC;
~~~~

~~~~
SELECT ordernumber,orderlinenumber,
quantityOrdered * priceEach AS subtotal 
FROM orderdetails
ORDER BY ordernumber,orderLineNumber, subtotal;
~~~~

NOTA 1 : Multiplica la cantidad de órdenes por el precio y muestra una columna cono subtotal: 

> quantityOrdered * priceEach AS subtotal

__EJERCICIOS__

* Mostrar la lista de los productos ordenados por el stock disponible del más grande al más pequeño:

~~~~
SELECT productname, quantityInStock  
FROM products 
ORDER BY quantityInStock DESC;
~~~~



























