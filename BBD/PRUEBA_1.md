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
WHERE c.country = ‘Chile’ 
AND c.creditlimit > 100;

~~~~
