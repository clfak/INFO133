## INFO133: Consultas SGBD Relacionales

* Importar la base de datos 'sakila'

> mysql -u root -p

<code> source sakila-schema.sql </code>

<code> source sakila-data.sql </code>

* ¿Cuáles son las películas sin actores?

<code> codigo </code>
~~~~
SELECT film.film_id, film.title
FROM film 
LEFT JOIN film_actor USING (film_id)
WHERE film_actor_id IS NULL;
~~~~
