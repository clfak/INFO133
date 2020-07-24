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
FALTAAAA
SELECT a.actor_id, a.first_name, a.last_name, fa.film_id, f.title
FROM actor a
LEFT JOIN film_actor fa USING (actor_id)
LEFT JOIN film f USING (film_id)
LIMIT 2;
SELECT a.actor_id, a.first_name, a.last_name, fa.film_id, f.title FROM actor a LEFT JOIN film_actor fa USING (actor_id) LEFT JOIN film f USING (film_id) LIMIT 40;
~~~~

* Mostrar las peliculas arrendadas según el día de la semana:

~~~

~~~
