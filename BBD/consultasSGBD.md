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

SELECT 

<code> completar </code>

* Mostrar las peliculas arrendadas según el día de la semana:

~~~
SELECT f.title, DAYOFWEEK(rental_date) 
FROM film f 
INNER JOIN inventory i ON i.film_id=f.film_id 
INNER JOIN rental r ON  r.inventory_id=i.inventory_id;
~~~
* Los los 10 actores que han participado en más películas: 

~~~
SELECT a.first_name,a.last_name, count(*) AS cantidad_peliculas  
FROM actor a 
JOIN film_actor fa ON a.actor_id=fa.actor_id 
JOIN film f ON f.film_id=fa.film_id 
GROUP BY a.actor_id 
ORDER BY cantidad DESC 
LIMIT 10;
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

4. Mostrar los titulos y descripción de peliculas que contienen la palabra "dog" en su descripción: 

~~~~
SELECT title, description 
FROM film f
WHERE f.description 
LIKE '%dog%'
~~~~

5. Mostrar los titulos y descripción de peliculas que más fueron arrendadas, ordenadas de del número más grande al más pequeño:

~~~
SELECT f.title, f.description, count(*) AS masArrendadas 
FROM rental r INNER JOIN inventory i ON r.inventory_id=i.inventory_id 
INNER JOIN film f ON f.film_id=i.film_id 
GROUP BY f.film_id 
ORDER BY masArrendadas DESC;
~~~

6. Mostrar las peliculas que nunca fueron arrendadas entre dos fechas:
