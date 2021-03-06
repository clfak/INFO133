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

~~~
SELECT a.first_name, a.last_name 
FROM actor a 
LEFT JOIN film_actor fa USING (actor_id) 
WHERE fa.film_id is NULL;
~~~

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
* Mostrar los clientes de países que empiezan con la letra 'S':
~~~
SELECT c.first_name, c.last_name, co.country 
FROM customer c 
JOIN address a ON a.address_id=c.address_id 
JOIN city ci ON ci.city_id=a.city_id 
JOIN country co ON co.country_id=ci.country_id 
WHERE co.country LIKE'S%';
~~~
* Mostrar el top-10 de países con más clientes:
~~~
SELECT  count(*) AS clientes, c.country 
FROM country c JOIN city ci ON ci.country_id=c.country_id JOIN address a ON ci.city_id=a.city_id 
JOIN customer cu ON cu.address_id=a.address_id 
GROUP BY c.country_id 
ORDER BY clientes DESC 
LIMIT 10;
~~~
* Mostrar las 10 primeras películas alfabéticamente y el número de copias que se disponen de cada una de ellas: 

~~~
SELECT f.title, count(*) AS cantidad 
FROM inventory i 
JOIN film f ON f.film_id=i.film_id 
GROUP BY i.film_id 
ORDER BY f.title 
LIMIT 10;
~~~
* Mostrar todas las películas que ha alquilado el cliente Deborah Walker: 
~~~
SELECT f.title 
FROM film f 
JOIN inventory i ON f.film_id=i.film_id 
JOIN rental r ON r.inventory_id=i.inventory_id 
JOIN customer cm ON cm.customer_id=r.customer_id 
WHERE cm.first_name LIKE 'Deborah' AND cm.last_name LIKE 'Walker';
~~~
* Mostrar los 10 mejores clientes: 
~~~
SELECT count(*) AS cant_alquiler, cm.first_name, cm.last_name 
FROM customer cm 
JOIN rental r ON cm.customer_id = r.customer_id 
GROUP BY cm.customer_id 
ORDER BY cant_alquiler DESC 
LIMIT 10;
~~~
* Mostrar la popularidad de las categorías cinematográficas entre los clientes españoles:
~~~
SELECT cat.name,count(*) AS alquileres 
FROM category cat 
JOIN film_category fc ON cat.category_id=fc.category_id 
JOIN film f ON fc.film_id = f.film_id 
JOIN inventory i ON f.film_id = i.film_id 
JOIN rental r ON i.inventory_id=r.inventory_id 
JOIN customer cm ON r.customer_id=cm.customer_id 
JOIN address a ON cm.address_id=a.address_id 
JOIN city ci ON a.city_id=ci.city_id 
JOIN country ct ON ci.country_id=ct.country_id 
WHERE ct.country LIKE 'Spain' 
GROUP BY cat.category_id 
ORDER BY alquileres DESC;
~~~

* Mostrar los 10 actores más populares en Argentina: 

~~~
SELECT act.first_name, act.last_name, count(*) AS alquileres  
FROM actor act 
JOIN film_actor fma ON act.actor_id=fma.actor_id 
JOIN film f ON fma.film_id = f.film_id 
JOIN inventory i ON f.film_id = i.film_id  
JOIN rental r ON i.inventory_id=r.inventory_id  
JOIN customer cm ON r.customer_id=cm.customer_id  
JOIN address a ON cm.address_id=a.address_id  
JOIN city ci ON a.city_id=ci.city_id  
JOIN country ct ON ci.country_id=ct.country_id
WHERE ct.country LIKE 'Argentina'  
GROUP BY act.actor_id  
ORDER BY alquileres DESC 
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
WHERE f.description LIKE '%dog%'
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
~~~
SELECT f.title
FROM film f 
JOIN inventory i USING (film_id)
JOIN rental r USING(inventory_id)
WHERE rental_date NOT  BETWEEN  '2005-05-24' AND '2006-01-21'
GROUP BY film_id;
~~~

7. Mostrar el volumen de negocio (ver tabla Payment) asociado a cada pelicula entre dos fechas, ordenado del volumen de negocio más grande al más pequeño: 
~~~
SELECT f.title,SUM(p.amount) as Volumen 
FROM film f
JOIN inventory i ON f.film_id=i.film_id
JOIN rental r ON i.inventory_id=r.inventory_id 
JOIN payment p on r.rental_id=p.rental_id 
WHERE r.rental_date BETWEEN  '2005-05-24' AND '2005-06-01'
GROUP BY f.film_id
ORDER BY Volumen DESC;
~~~
8. Mostrar título de la pelicula que generó más volumen de negocio:
~~~
SELECT f.title,SUM(p.amount) as Volumen 
FROM film f
JOIN inventory i ON f.film_id=i.film_id
JOIN rental r ON i.inventory_id=r.inventory_id 
JOIN payment p on r.rental_id=p.rental_id 
GROUP BY f.film_id
ORDER BY Volumen DESC
LIMIT 1;
~~~

9. Mostrar los títulos de pelicula que generaron más de cierto valor de volumen de negocio: 
~~~
SELECT f.title,SUM(p.amount) as Volumen 
FROM film f
JOIN inventory i ON f.film_id=i.film_id
JOIN rental r ON i.inventory_id=r.inventory_id 
JOIN payment p on r.rental_id=p.rental_id 
GROUP BY f.film_id
HAVING Volumen >100
ORDER BY Volumen DESC;
~~~

10. Contar cuántos Staff distintos han arrendados la pelicula que generó más volumen de negocio: 
~~~
SELECT count(DISTINCT r.staff_id) AS n_staff
FROM rental r
JOIN inventory i ON r.inventory_id=i.inventory_id
WHERE i.film_id = (SELECT f.film_id
                   FROM film f
                   JOIN inventory i ON i.film_id=f.film_id
                   JOIN rental r ON r.inventory_id=i.inventory_id
                   JOIN payment p ON p.rental_id=r.rental_id
                   GROUP BY f.film_id
                   ORDER BY SUM(p.amount) DESC
                   LIMIT 1);
 ~~~
 
 11. Mostrar la pelicula más arrendada (titulo) por un Staff particular: 

~~~
SELECT f.title, r.staff_id, count(*) as Cantidad_Max
FROM film f
JOIN inventory i USING(film_id)
JOIN rental r USING (inventory_id)
WHERE staff_id = 1
GROUP BY r.staff_id, f.film_id
ORDER BY Cantidad_Max DESC
LIMIT 1;
~~~

12. Mostrar la pelicula más arrendada (titulo) por cada uno de los miembros del Staff:
~~~
(SELECT f.title, r.staff_id, count(*) as Cantidad_Max
FROM film f
JOIN inventory i ON f.film_id=i.film_id
JOIN rental r ON i.inventory_id=r.inventory_id
WHERE r.staff_id = 1
GROUP BY r.staff_id, f.film_id
ORDER BY Cantidad_Max DESC
LIMIT 1)
UNION
(SELECT f.title, r.staff_id, count(*) as Cantidad_Max
FROM film f
JOIN inventory i ON f.film_id=i.film_id
JOIN rental r ON i.inventory_id=r.inventory_id
WHERE r.staff_id = 2
GROUP BY r.staff_id, f.film_id
ORDER BY Cantidad_Max DESC
LIMIT 1);
~~~
13. Mostrar la evolución del volumen de negocio mensual de una tienda dada
~~~
SELECT YEAR(rental_date) as Año, MONTH(rental_date) AS Mes, SUM(p.amount) AS Volumen
FROM rental r 
JOIN payment p ON p.rental_id=r.rental_id
JOIN staff s ON s.staff_id=p.staff_id
JOIN store st ON st.store_id=s.store_id
WHERE st.store_id = 2
GROUP BY Mes, Año
ORDER BY Año;
~~~

14. ¿Cuáles son los actores que más generan volumen de negocio?
~~~
SELECT a.first_name, a.last_name, SUM(p.amount) as Volumen
FROM actor a
JOIN film_actor USING (actor_id)
JOIN film USING (film_id)
JOIN inventory USING(film_id)
JOIN rental USING(inventory_id) 
JOIN payment p USING(rental_id) 
GROUP BY a.actor_id
ORDER BY Volumen DESC
LIMIT 10;
~~~
15. ¿Cuáles son los actores que jugaron más en pelicula de tipo "Sci-Fi"?
~~~
SELECT a.first_name, a.last_name, count(*) AS Veces 
FROM actor a  
JOIN film_actor fa ON fa.actor_id=a.actor_id 
JOIN film f ON f.film_id=fa.film_id 
JOIN film_category fc ON fc.film_id=f.film_id 
JOIN category c ON fc.category_id=c.category_id 
WHERE c.name LIKE 'Sci-Fi' 
GROUP BY a.actor_id,fc.category_id 
ORDER BY Veces DESC
LIMIT 10;
~~~

16. ¿Cuáles son los actores que nunca jugaron en pelicula de tipo "Sci-Fi"?
~~~
SELECT DISTINCT a.first_name, a.last_name
FROM actor a  
JOIN film_actor fa ON fa.actor_id=a.actor_id 
JOIN film f ON f.film_id=fa.film_id 
JOIN film_category fc ON fc.film_id=f.film_id 
JOIN category c ON fc.category_id=c.category_id 
WHERE c.name NOT LIKE 'Sci-Fi' 
GROUP BY a.actor_id,fc.category_id
ORDER BY last_name;
~~~

18. Contar el número de peliculas promedio por actores: 

~~~
SELECT AVG(Movie)
FROM (SELECT count(*) as Movie
	     FROM actor a
	     JOIN film_actor fi ON fi.actor_id = a.actor_id
	     JOIN film f ON f.film_id = fi.film_id
	     GROUP BY first_name, last_name
	     ORDER BY Movie DESC) as t1;
~~~


