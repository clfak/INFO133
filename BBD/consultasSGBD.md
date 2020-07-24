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
