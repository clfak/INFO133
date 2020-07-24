## INFO133: Consultas SGBD Relacionales

> mysql -u root -p

* Importar la base de datos 'sakila'

<code> source sakila-schema.sql </code>

<code> source sakila-data.sql </code>

* ¿Cuáles son las películas sin actores?

~~~~
SELECT f.film_id, f.title
FROM film f
LEFT JOIN film_actor USING (film_id)
WHERE film_actor.film_id IS NULL;
~~~~

* ¿Cuáles son los actores sin peliculas?

<code> completar </code>

* Mostrar las peliculas arrendadas según el día de la semana:

~~~
SELECT f.title, DAYOFWEEK(rental_date) 
FROM film f 
INNER JOIN inventory i ON i.film_id=f.film_id 
INNER JOIN rental r ON  r.inventory_id=i.inventory_id;
~~~

---
__Ejercicios:__

1. Mostrar el 'firstname' y 'lastname' de los clientes activos (active):

~~~
SELECT first_name, last_name
FROM customer 
WHERE active=true;
~~~

2. Contar el número de clientes activos por paises:
~~~
SELECT c.country,count(*) AS cantidad_activos
FROM country c INNER JOIN city ci ON ci.country_id=c.country_id 
INNER JOIN address a ON ci.city_id=a.city_id 
INNER JOIN customer cm ON cm.address_id=a.address_id 
WHERE cm.active=true 
GROUP BY c.country_id 
ORDER BY c.country ASC;
~~~

3. Mostrar los paises dónde hay al menos 10 clientes activos, ordenados del número más grande al más pequeño: 
~~~
SELECT c.country,count(*) AS cantidad_activos
FROM country c INNER JOIN city ci ON ci.country_id=c.country_id 
INNER JOIN address a ON ci.city_id=a.city_id 
INNER JOIN customer cm ON cm.address_id=a.address_id 
WHERE cm.active=true 
GROUP BY c.country_id 
HAVING cantidad_activos >= 10
ORDER BY cantidad_activos DESC;
~~~

4. Mostrar los titulos y descripción de peliculas que contienen la palabra "dog" en du descripción: 

~~~~

~~~~






