## INFO133: Consultas base de datos Classicmodels

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
