---


---

<h1 id="desafío-----postgresql--avanzado">Desafío  -  PostgreSQL  Avanzado</h1>
<p>En  esta  actividad  trabajaremos  con  las  diferentes  queries  desde  el  terminal  de  postgres.</p>
<p>Para  desarrollar  esta  actividad,  tendrán  que  anotar  cada  una  de  las  queries  que  utilizaron  en  un archivo  txt  o  md  y  subir  los  archivos  comprimidos  en  formato  zip  a  la  plataforma  Empieza.</p>
<p>Deben  también  ingresar,  al  final  de  este  archivo  (txt),  el  nombre  de  los  integrantes  del  grupo  que participaron  en  el  desarrollo  de  esta  actividad.  (Cada  integrante  debe  subir  el  archivo  a  la plataforma).</p>
<h2 id="descripción">Descripción</h2>
<p>Se  busca  desarrollar  una  aplicación  llamada  Pintagram.  Esta  aplicación  debe  permitir  a  los  usuarios subir  imágenes  y  que,  a  su  vez,  estas  imágenes  pertenezcan  a  ese  usuario.  Además  los  usuarios podrán  darle  like  a  las  imágenes  de  otros  usuarios.  Cada  una  de  las  imágenes  tendrá  varios  tags  y cada  uno  de  esos  tags  podrá  referenciar  a  varias  imágenes.  En  este  ejercicio  se  tomarán  en  cuenta las  consultas  a  2  o  más  tablas  y  los  constrains  al  momento  de  la  creación  de  la  tabla.</p>
<h2 id="restricciones--constraints">Restricciones  (constraints)</h2>
<p>En  esta  parte  del  ejercicio  se  debe  investigar  como  aplicar  relgas  a  la  base  de  datos  para  que  se cumpla:</p>
<ol>
<li>
<p>Un  usuario  solo  puede  darle  like  1  vez  a  cada  imagen.</p>
</li>
<li>
<p>Una  imagen  no  puede  tener  tags  repetidos.</p>
</li>
</ol>
<p>Crear  base  de  datos  en  base  a  los  requerimientos  indicados.</p>
<h2 id="checkpoints">Checkpoints</h2>
<ol>
<li>Antes  de  empezar  a  crear  la  base  de  datos  deben  leer  todas  las  instrucciones,  modelar  la base  y  generar  un  diagrama  que  tendrán  que  adjuntar  a  las  respuestas  de  este  ejercicio.</li>
</ol>
<p><img src="https://drive.google.com/uc?export=view&amp;id=1PcBBV_tDOuRIX441xGwJLqYOjZeTXoH8" alt="pintagram_db"></p>
<pre><code>	CREATE DATABASE pintagram;
	
	\c pintagram;

	CREATE TABLE users ( 
		ID SERIAL PRIMARY KEY, 
		username VARCHAR(20)
	);
	
	SELECT * FROM users;
	 id | username 
	----+----------
	(0 rows)
	
	CREATE TABLE images (
		id SERIAL PRIMARY KEY, 
		name VARCHAR(20), 
		user_id INTEGER REFERENCES users(id)
	);
	
	SELECT * FROM images;
	 id | name | user_id 
	----+------+---------
	(0 rows)


	CREATE TABLE likes (
		PRIMARY KEY(user_id, img_id), 
		user_id INTEGER REFERENCES users(id), 
		img_id INTEGER REFERENCES images(id)
	);
	
	SELECT * FROM likes;
	 user_id | img_id 
	---------+--------
	(0 rows)
	
	CREATE TABLE tags (
		id SERIAL PRIMARY KEY, 
		name VARCHAR(10)
	);
	
	SELECT * FROM tags;
	 id | name 
	----+------
	(0 rows)

	ALTER TABLE tags ADD CONSTRAINT name UNIQUE(name);
	

	CREATE TABLE img_tags (
		PRIMARY KEY(img_id, tag_id), 
		img_id INTEGER REFERENCES images(id), 
		tag_id INTEGER REFERENCES tags(id)
	);
	
	SELECT * FROM img_tags;
	 img_id | tag_id 
	--------+--------
	(0 rows)

	INSERT INTO users (username) VALUES ('user_1');
	INSERT INTO users (username) VALUES ('user_2');
	INSERT INTO users (username) VALUES ('user_3');

	SELECT * FROM users;
	 id | username 
	----+----------
	  1 | user_1
	  2 | user_2
	  3 | user_3
	(3 rows)
</code></pre>
<ol start="2">
<li>
<p>Ingresar  2  imágenes  por  usuario.</p>
<pre><code>INSERT INTO images (name, user_id) VALUES ('gato acostado', 1); 
INSERT INTO images (name, user_id) VALUES ('gato enojado', 1);

INSERT INTO images (name, user_id) VALUES ('perro corriendo', 2);
INSERT INTO images (name, user_id) VALUES ('perro comiendo', 2);

INSERT INTO images (name, user_id) VALUES ('tortuga', 3);
INSERT INTO images (name, user_id) VALUES ('hamster', 3);

SELECT * FROM images;
 id |      name       | user_id 
----+-----------------+---------
  1 | gato acostado   |       1
  2 | gato enojado    |       1
  4 | perro corriendo |       2
  5 | perro comiendo  |       2
  7 | tortuga         |       3
  8 | hamster         |       3
(6 rows)
</code></pre>
</li>
<li>
<p>Ingresar  3  likes  por  cada  imagen.</p>
<pre><code>INSERT INTO likes (user_id, img_id) VALUES (1, 1);
INSERT INTO likes (user_id, img_id) VALUES (1, 2);
INSERT INTO likes (user_id, img_id) VALUES (1, 4);
INSERT INTO likes (user_id, img_id) VALUES (1, 5);
INSERT INTO likes (user_id, img_id) VALUES (1, 7);
INSERT INTO likes (user_id, img_id) VALUES (1, 8);

INSERT INTO likes (user_id, img_id) VALUES (2, 1);
INSERT INTO likes (user_id, img_id) VALUES (2, 2);
INSERT INTO likes (user_id, img_id) VALUES (2, 4);
INSERT INTO likes (user_id, img_id) VALUES (2, 5);
INSERT INTO likes (user_id, img_id) VALUES (2, 7);
INSERT INTO likes (user_id, img_id) VALUES (2, 8);

INSERT INTO likes (user_id, img_id) VALUES (3, 1);
INSERT INTO likes (user_id, img_id) VALUES (3, 2);
INSERT INTO likes (user_id, img_id) VALUES (3, 4);
INSERT INTO likes (user_id, img_id) VALUES (3, 5);
INSERT INTO likes (user_id, img_id) VALUES (3, 7);
INSERT INTO likes (user_id, img_id) VALUES (3, 8);

SELECT * FROM likes;
 user_id | img_id 
---------+--------
       1 |      1
       1 |      2
       1 |      4
       1 |      5
       1 |      7
       1 |      8
       2 |      1
       2 |      2
       2 |      4
       2 |      5
       2 |      7
       2 |      8
       3 |      1
       3 |      2
       3 |      4
       3 |      5
       3 |      7
       3 |      8
(18 rows)
</code></pre>
</li>
<li>
<p>Ingresar  8  tags.</p>
<pre><code>INSERT INTO tags (name) VALUES ('gatos');
INSERT INTO tags (name) VALUES ('perros');
INSERT INTO tags (name) VALUES ('mascotas');
INSERT INTO tags (name) VALUES ('hamsters');
INSERT INTO tags (name) VALUES ('tortugas');
INSERT INTO tags (name) VALUES ('cute');
INSERT INTO tags (name) VALUES ('otrotag');
INSERT INTO tags (name) VALUES ('ultimotag');

SELECT * FROM tags;
 id |   name    
----+-----------
  1 | gatos
  2 | perros
  3 | mascotas
  4 | hamsters
  5 | tortugas
  6 | cute
  7 | otrotag
  8 | ultimotag
(8 rows)
</code></pre>
</li>
<li>
<p>Ingresar  3  tags  por  imagen.</p>
<pre><code>INSERT INTO img_tags (img_id, tag_id) VALUES (1, 1);
INSERT INTO img_tags (img_id, tag_id) VALUES (1, 3);
INSERT INTO img_tags (img_id, tag_id) VALUES (1, 6);

INSERT INTO img_tags (img_id, tag_id) VALUES (2, 1);
INSERT INTO img_tags (img_id, tag_id) VALUES (2, 3);
INSERT INTO img_tags (img_id, tag_id) VALUES (2, 6);

INSERT INTO img_tags (img_id, tag_id) VALUES (4, 2);
INSERT INTO img_tags (img_id, tag_id) VALUES (4, 3);
INSERT INTO img_tags (img_id, tag_id) VALUES (4, 6);

INSERT INTO img_tags (img_id, tag_id) VALUES (5, 2);
INSERT INTO img_tags (img_id, tag_id) VALUES (5, 3);
INSERT INTO img_tags (img_id, tag_id) VALUES (5, 6);

INSERT INTO img_tags (img_id, tag_id) VALUES (7, 5);
INSERT INTO img_tags (img_id, tag_id) VALUES (7, 6);
INSERT INTO img_tags (img_id, tag_id) VALUES (7, 7);

INSERT INTO img_tags (img_id, tag_id) VALUES (8, 4);
INSERT INTO img_tags (img_id, tag_id) VALUES (8, 6);
INSERT INTO img_tags (img_id, tag_id) VALUES (8, 8);

SELECT * FROM img_tags;
 img_id | tag_id 
--------+--------
      1 |      1
      2 |      1
      1 |      3
      1 |      6
      2 |      3
      2 |      6
      4 |      2
      4 |      3
      4 |      6
      5 |      2
      5 |      3
      5 |      6
      7 |      5
      7 |      6
      7 |      7
      8 |      4
      8 |      6
      8 |      8
(18 rows)
</code></pre>
</li>
<li>
<p>Crear  una  consulta  que  muestre  el  nombre  de  la  imagen  y  la  cantidad  de  likes  que  tiene  esa imagen.</p>
<pre><code>SELECT images.name, count(likes.img_id) AS likes FROM images RIGHT JOIN likes ON (images.id = img_id) GROUP BY images.name;
      name       | likes 
-----------------+-------
 gato enojado    |     3
 perro comiendo  |     3
 tortuga         |     3
 hamster         |     3
 perro corriendo |     3
 gato acostado   |     3
(6 rows)
</code></pre>
</li>
<li>
<p>Crear  una  consulta  que  muestre  el  nombre  del  usuario  y  los  nombres  de  las  fotos  que  le pertenecen.</p>
<pre><code>SELECT users.username, images.name FROM users RIGHT JOIN images ON (users.id = user_id);
 username |      name       
----------+-----------------
 user_1   | gato acostado
 user_1   | gato enojado
 user_2   | perro corriendo
 user_2   | perro comiendo
 user_3   | tortuga
 user_3   | hamster
(6 rows)
</code></pre>
</li>
<li>
<p>Crear  una  consulta  que  muestre  el  nombre  del  tag  y  la  cantidad  de  imagenes  asociadas  a  ese tag.</p>
<pre><code>SELECT tags.name, count(img_tags.tag_id) FROM tags RIGHT JOIN img_tags ON (tags.id = tag_id) GROUP BY tags.name;
   name    | count 
-----------+-------
 gatos     |     2
 perros    |     2
 mascotas  |     4
 hamsters  |     1
 ultimotag |     1
 otrotag   |     1
 cute      |     6
 tortugas  |     1
(8 rows)
</code></pre>
</li>
</ol>

