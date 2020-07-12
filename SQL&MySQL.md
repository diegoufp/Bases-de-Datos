# SQL & MySQL

## Base de Datos

Es un sistema donde se puede guardar datos puntuales de cualquier cantidad de cosas. Los datos de estos sistemas se convierten en información, a su vez la información se convierte en operaciones de negocios, y estás operaciones pueden servir como medio para algún otro bien como el dinero.

### Modelo relacional
Se basa en un modelo donde se unen tablas que pueden llegar a tener conexiones o relaciones entre ellas.
Un ejemplo puede ser una base de datos que almacene información de trabajadores de una empresa y su perfil medico, aquí vamos a tener una tabla con los datos de los trabajadores, y a su vez una tabla con los perfiles médicos, cada perfil medico estará relacionado a un trabajador en especifico.

### Importancia de las relaciones entre tablas

Gracias a que podemos establecer relaciones entre las tablas y sus datos somos capaces de extraer más información.

## La diferencia entre SQL y MySQL:

- **SQL** es un **lenguajes de programación orientado a consultas** de bases de datos (Structured Query Language)

- **MySQL** es un sistema de administración de bases de datos (Database Management System, DBMS) o también llamado motor de bases de datos.

Existen muchos tipos de bases de datos, desde un simple archivo hasta sistemas relacionales orientados a objetos. MySQL, como base de datos relacional, utiliza múltiples tablas para almacenar y organizar la información.

Fue escrito en C y C++ y se integra perfectamente con los lenguajes de programación más usados en todo el mundo.

## Inicios de MySQL

### Iniciar MySQL

La forma de conectarnos a nuestro servidor MySQL a través de nuestra terminal podría ser:

```
mysql -h <dirección_de_nuestro_servidor> -u <usuario> -pmy_password -P <puerto>
```

esta es una forma muy insegura de conexión ya que estaríamos dejando la password escrita en texto plano… cualquiera con acceso a nuestro usuario podría obtenerla simplemente ejecutando el comando:

```
history
```

para esto el comando ‘mysql’ nos permite la opción de indicarle que debemos usar una contraseña, pero que queremos que nos la pregunte de forma segura a través de nuestro terminal. Para hacer esto el solo debemos indicarle el argumento ‘-p’; quedaría así:

```
mysql -h <dirección_de_nuestro_servidor> -u <usuario> -p -P <puerto>
Enter password: <aquíintroduciríastupassword>?
```

Una situación muy normal en entornos de producción es tener que acceder a nuestro servidor a través de un túnel SSH, es decir, primero accedemos a una máquina que podría ser la entrada de nuestro cluster y después, desde ahí, accederíamos a nuestro servidor MySQL.

Para hacer esto primero crearíamos nuestro túnel ssh y después nos conectaríamos a nuestro servidor MySQL de la siguiente forma:

```
ssh user@ssh.example.com -L 3307:mysql1.example.com:3306 -N -f
```

```
mysql -h 127.0.0.1 -P 3307
```

de esta forma lo que le estamos diciendo a nuestro ordenador es:

Todo lo que yo envié al puerto 3307 de mi máquina local, saldrá por la máquina ssh.example.com (que ya esta dentro del cluster) en dirección a la máquina mysql1.example.com que escucha en el puerto 3306 (puerto por defecto).

Ahora, me conector a través de mi ordenador (la IP 127.0.0.1 es mi pc) en el puerto 3307

Ahora ya te puedes conectar a tu servidor, no expuesto en internet, desde tu pc local.

Si hacéis esto último tenéis que tener en cuenta dos cosas:

- La opción ‘-f’ en el comando que crea el túnel ssh, esta diciéndole a la máquina que deje el túnel creado y funcionando. 

Pero… ¿Y cómo cierro el túnel? Aquí hay dos opciones:

- Lo ejecutas sin la opción ‘-f’ en unaterminal, haces en otra terminal la conexión y lo que tengas que hacer y cuando acabes haces Ctrl+C para matar el comando y cerrar el túnel.

Lo ejecutas con la opción -f y cuando lo quieras cerrar haces lo siguiente: (el comando ps con estos argumentos funciona bien en linux. 

```
ps -aux | grep ssh
```

buscar el PID en la salida del comando que será de esta forma o similar:

```
USERPID %CPU %MEMVSZRSSTTYSTATSTARTTIMECOMMAND
root         1  0.0  0.0  10440   588 ?        Ss   00:28   0:00sshuser@ssh.example.com -L 3307:mysql1.examp...
```

y matas el proceso que se quedo corriendo el background de esta forma:

```
kill -9 <PID>
```

### Comandos iniciales

- Lista de bases de datos que tiene el servidor:

```
SHOW databases;
```

- Seleccionar la base de datos a trabajar:

```
USE name_database;
```

- Mostrar las tablas que contiene la base de datos:

```
SHOW tables;
```

- Muestra cual es la base de datos que tenemos seleccionada o en la que se esta trabajando.

```
SELECT database();
```

## Principales tipos de tablas

### MyISAM

1. Bloqueo de tabla

2. Aumento del rendimiento si nuestra aplicación realiza un elevado número de consultas “Select”.

3. Las tablas pueden llegar a dar problemas en la recuperación de datos.

4. Permite hacer búsquedas full-text

5. Menor consumo memoria RAM

6. Mayor velocidad en general a la hora de recuperar datos.

7. Ausencia de características de atomicidad ya que no tiene que hacer comprobaciones de la integridad referencial, ni bloquear las tablas para realizar las operaciones, esto nos lleva como los anteriores puntos a una mayor velocidad.

8. Recomendable para aplicaciones en las que dominan las sentencias SELECT antes que INSERT/ UPDATE.

### InnoDB

1. Bloqueo de registros

2. Soporte de transacciones

3. Rendimiento

4. Concurrencia

5. Confiabilidad

6. Nos permite tener las caracteristicas ACID (Atomicity, Consistency, Isolation and Durability), garantizando la integridad de nuestras tablas.

7. Integridad de datos, cuando los contenidos se modifican con sentencias INSERT, DELETE OP UPDATE, la integridad de los datos almacenados puede perderse de muchas maneras diferentes.

8. InnoDB se recupera de errores o reinicios no esperados del sistema a partir de sus logs, mientras que MyISAM requiere una exploracion, reparacion y reconstruccion de indices de los datos de las tablas que aun no habian sido volcadas a disco.

9. Ademas es probable que si nuestra aplicacion hace un uso elevado de INSERT y UPDATE notemos un aumento de rendimiento con respecto a MyISAM.

10. Permite hacer búsquedas full-text (mysql >= 5.6)

## Creacion de tablas

### [CREATE](https://github.com/diegoufp/Bases-de-Datos#ddl-create "CREATE")

- Crear una tabla:

```sql
CREATE database blogpost;
```

- Crear la tabla si aun no existe:

```sql
CREATE DATABASE IF NOT EXISTS blogpost;
```

- Ver los warning:

El error vs Warnings: la diferencia entre estos dos es que el error rompe cualquier flujo de trabajo que tengamos en nuestra aplicación mientras que el warnnigs nos muestra una advertencia que no rompe el flujo de trabajo workflow.

```
SHOW WARNINGS;
```
- Ver las columnas de nuestra tabla:

```sql
DESCRIBE authors;
```

```sql
DESC authors;
```

- Describe la estructura de la bases de datos incluyendo más información:

```sql 
SHOW FULL COLUMNS FROM books;
```
- Al hacer un select from where cerrar con \G para presentación más legible.

```sql
SELECT * FROM clients WHERE client_id = 4\G
```

- Mostrar 10 resuldados aleatorios, pero no es eficiente computacionalmente.
```sql
SELECT * FROM authors ORDER BY RAND() LIMIT 10;
```

- Contar puedes contar con COUNT(*), pero una buena practica es se lo mas especificos posibles, en vez de asterisco podrias poner el id.
```sql
SELECT COUNT(book_id) FROM books; 

### PRACTICA CREANDO UNA DATABASE DE LIBROS

Una buena practica es que cada tabla se llame en el plural de la entidad que vamos a guardar.

En una base de datos no se guardan imagenes, lo que se podria hacer es que en un varchar se podria guardar la url de donde la imagen esta almacenada.

#### ID

Tambien es necesario ubicar a cada tupla de manera unica es por eso que en una tabla es necesario un [**id**](https://github.com/diegoufp/Bases-de-Datos#constraints-restricciones "PRIMARY KEY").

MySQL da la posibilidad de hacer a un entero auto incremental (AUTO_INCREMENT), lo que quiere decir es que el entero aumentara en uno sobre el valor anterior, esto tambien quiere decir que si borramos una tupla el auto incremento no remplazara el valor borrado, seguira su sucesion normal.

Tambien existe la posibilidad de que un entero sea solo positivo, es decir que no se almacenaria el signo en el campo del bit por lo cual no estariamos ahorrando bits para darle mayor capacidad y esto se hace con [**INTEGER**](https://github.com/diegoufp/Bases-de-Datos#n%C3%BAmeros "Numeros") **UNSIGNED**.

#### [Constraints](solo aceptará enteros positivos "Constraints") 

- **INTEGER**: Es un entero,Ocupa 4 bytes.

- **AUTO_INCREMENT**: Hace enteros que se auto incrementan, se meten a una dupla y detecta el numero para hacerlo crecer automáticamente. *Al borrar un dato de MYSQL no lo nota y sigue aumentando números, EJEMPLO 1 2 4, si se elimina el 3 no le importa y sigue contando.

- **KEY UPDATE**: cuando encuentre un duplicado, actualiza con el nuevo valor que envió.

- **IGNORE ALL**: si hay error ignore y ejecute el comando. **NO USAR NUNCA ESTE COMANDO**

- **UNSIGNED**: solo aceptará enteros positivos.

- **TINYINT**: Ocupa 1 byte.

- **SMALLINT**: Ocupa 2 byes.

- **BETWEEN**: Sirve para numero y para fechas, sirve como minitador.

- **Varchar**: Tu le tienes que indicar el numero de caracteres a almacenar de texto.EJEMPLO VARCHAR(2) SI.

- **NOTNULL**: Es decirle a MYSQL que no almacene algo ,algo nulo es que no existe información, en otras palabras es nada.

- **DEFAULT**: Es que no le mando información por defecto debe poner el valor que se agrega después del mismo.

- **COMMENT**: Comentario a la columna, que solo se ve al revisar el código.

- **DOUBLE**: Almacena los números enteros y los decimales. *DOUBLE (6,2) esto hace que de los 6 números que le estamos diciendo 2 de ellos los use solo para decimales. 

- **FLOAT**: Es un numero que esta almacenando hasta 6 decimales, es para cálculos precisos.

- **TEXT**: Es meter tanto como se pueda. EJEMPLO Descripción de un libro

- **DIFERENTE**: diferente en mysql es <> no es !=

- **ENUM**: Es una enumeracion de datos, es decir que yo le voy a decir a la base de datos cuales son las opciones que puede tomar esta columna.

- **TIMESTAMP**:
Está basado en el número epoch que es el 1 enero de 1970 hasta la fecha y es donde se determina el inicio de las computadoras y es un número entero que se guarda en segundos y permite hacer operaciones sobre el.

- **DATETIME**:
Este tipo de datos puede guardar cualquier valor de tipo fecha sin restricción. Incluso anterior a nuestra era. es por eso que las fechas de nacimiento de usuarios debe utilizar este valor para garantizar que podemos registrarlos con la fecha adecuada.

- **TIMESTAMP vs DATETIME**: 
  * TIMESTAMP “NO PUEDE HACER TODO LO DE DATETIME pero DATETIME SÍ PUEDE HACERLO DE UN TIMESTAMP”, 
  * DATETIME no está guardado en segundos y no es tan eficiente para hacer cálculos.

- **created_ad**:
 Es una columna de buena práctica que permite saber cuando se creó un registro. Está utilizará un conjunto de propiedades llamada entre ella se colocará DEFAULT CURRENT_TIMESTAMP . Cuando se realiza un insert sí el valor de esta columna viene vacío colocará en la tupla el valor de la fecha en que se creó de manera automática.

 ```sql
 `created_at` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
 ```

- **created_ad y updated_ad**:
Es buena práctica tener una columna que permite saber el momento exacto en el que se crea un registro o se actualiza. Este tipo de dato se comporta más como una meta-información y nos puede ayudar por ejemplo a cuántos usuarios fueron creados en una fecha en específico, saber cuando una tupla se actualizó.

```sql
`updated_at` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
```

- **NINGUNA TUPLA SE ELIMINA**:
Es buena práctica no eliminar registros de una bases de datos es por ello que se crea una columna como active que es un valor booleano dicho valor sirve para para decir si el registro está activo o no. Ejemplo:

```sql
active TINYINT(1) NOT NULL DEFAULT 1
```
### Ejemplo de la informacion

```sql
CREATE TABLE IF NOT EXISTS `books` (
  `book_id` INTEGER UNSIGNED PRIMARY KEY NOT NULL AUTO_INCREMENT, `author_id` INTEGER UNSIGNED, `title` VARCHAR(100) NOT NULL, `year` INTEGER UNSIGNED NOT NULL DEFAULT '1900', `language` VARCHAR(2) NOT NULL DEFAULT 'es' COMMENT 'ISO 639-1 Language', `cover_url` varchar(500), `price` DOUBLE(6,2) NOT NULL DEFAULT 10.0, `sellable` TINYINT(1) DEFAULT '1', `copies` INTEGER NOT NULL DEFAULT '1', `description` TEXT
);
```

```sql
CREATE TABLE IF NOT EXISTS `authors` (
  `author_id` INTEGER UNSIGNED PRIMARY KEY AUTO_INCREMENT, `name` VARCHAR(100) NOT NULL, `nationality` VARCHAR(3)
);
```

```sql
CREATE TABLE clients (
  `client_id` INTEGER UNSIGNED PRIMARY KEY AUTO_INCREMENT, `name` VARCHAR(50) NOT NULL,
  `email` VARCHAR(100) NOT NULL UNIQUE,
  `birthdate` DATETIME,
  `gender` ENUM('M', 'F', 'ND') NOT NULL,
  active TINYINT(1) NOT NULL DEFAULT 1,
  `created_at` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP, `updated_at` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

```sql
CREATE TABLE IF NOT EXISTS operations(
  `operation_id` INTEGER UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  `book_id` INTEGER UNSIGNED,
  `client_id` INTEGER UNSIGNED,
  `type` ENUM('P', 'D', 'V'),
  `created_at` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  `finshed` TINYINT(1) NOT NULL DEFAULT '0'
);
```
#### INSERT

Formas en la que se puede usar el INSERT.

- Depende de la configuracion puede que te marque un warning por dejar un valor vacio.

```sql
INSERT INTO authors(`author_id`, `name`, `nationality`) VALUES('','Juan Rulfo','MEX');
```

```sql
INSERT INTO authors(`name`, `nationality`) VALUES('Gabriel Garcia Marquez', 'COL');
```

```sql
INSERT INTO authors VALUES ('', 'Juan Gabriel Vasquez', 'COL');
```
- Si tienes una lista de n cantidad de datos que quieras insertar en una sola table, no necesitas hacer una sentencia cada vez.
Una recomendacion es hacer inserciones de 50, esto por si llegara a haber una problema con la conexion no se perderian tantos datos, se puede establecer un numero maximo en la configuracion.

```sql
INSERT INTO authors(`name`, `nationality`) VALUES ('Julio Cortazar', 'ARG'), ('Isabel Allende', 'CHI'), ('Octavio Paz', 'MEX'), ('Juan Carlos Onetti', 'URU');
```

```sql
INSERT INTO`clients`(client_id, name, email, birthdate, gender, active) VALUES
	(1,'Maria Dolores Gomez','Maria Dolores.95983222J@random.names','1971-06-06','F',1),
	(2,'Adrian Fernandez','Adrian.55818851J@random.names','1970-04-09','M',1),
	(3,'Maria Luisa Marin','Maria Luisa.83726282A@random.names','1957-07-30','F',1),
	(4,'Pedro Sanchez','Pedro.78522059J@random.names','1992-01-31','M',1);
```

```sql
INSERTINTO clients(name, email, birthdate,gender, active) VALUES ('Pedro Sanchez','Pedro.78522059J@random.names','1992-01-31','M',1);
```
- **ON DUPLICATE KEY**: En dato duplicado que debe hacer. Se explican dos opciones

    * **IGNORE ALL**: si hay error ignore y ejecute el comando. no usar nunca
    * **KEY UPDATE**: cuando encuentre un duplicado, actualiza con el nuevo valor que envió.

```sql
INSERTINTO clients(name, email, birthdate, gender, active) VALUES 
('Pedro Sanchez','Pedro.78522059J@random.names','1992-01-31','M',0)
ON DUPLICATE KEY UPDATE active = VALUES(active);
```

#### Inserción de datos usando queries anidados

Supongamos que insertamos un libro, pero como el autor es de tipo integer, por referencia lógica a la tabla authors, en un primer momento tenemos que hacer un select a la tabla authors, fijarse que id tiene el autor en cuestión y regresar a insertar el registro.

```sql
INSERT INTO books(title, author_id) VALUES ('El Laberinto de la Soledad', 6);
```

Sin embargo al usar INSERT con SELECT anidados, podemos saltarnos el paso de hacer el select previo a la tabla de autores y colocarlo internamente en la consulta de insercción

Para el campo author_id, su valor lo obtengo de una subconsulta, de ahí que este entre parentesis, para que se haga primero.

```sql
INSERT INTO books (title, author_id, year) VALUES ('Vuelta al laberinto de la Soledad', (SELECT author_id FROM authors WHERE name = 'Octavio Paz' LIMIT 1), 1960);
```

LIMIT 1; Esta sentencia limita sólo traer la primera ocurrencia. Esa sentencia sumamente importante cuando decidimos traer un registro.

Los sub-querys normalmente tienen una notación On una función exponencial.Los sub-querys pueden tardar sé demasiado y también puede insertar datos erróneos en el caso de los insert anidados.

manejese con cuidado los querys anidados.

## Bash y archivos SQL

Comando para ejecutar nuestro script SQL con el comando mysql:

```
mysql -u root -p < all_schema.sql
```

Ejecutar nuestro script directamante en una base de datos especifica (en este caso en “cursoplatzi” ):

```
mysql -u root -p -D cursoplatzi < al_data.sql
```

## Su majestad el SELECT

- Listar todas la tuplas de la tabla clients
```sql
SELECT * FROM clients;
```

- Listar todos los nombres de la tabla clients
```sql
SELECT name FROM clients;
```

- Listar todos los nombres, email y género de la tabla clients:
```sql
SELECT name, email, gender FROM clients;
```

- Listar los 10 primeros resultados de la tabla clients
```sql
SELECT name, email, gender FROM clients LIMIT 10;
```

- Listar todos los clientes de género Masculino
```sql
SELECT name, email, gender FROM clients WHERE gender = 'M';
```

- Listar el año de nacimientos de los clientes, con la función YEAR()
```sql
SELECTYEAR(birthdate) FROM clients;
```

- Mostrar el año actual
```sql
SELECT YEAR(NOW());
```

- Listar los 10 primeros resultados de las edades de los clientes
```sql
SELECT YEAR(NOW()) - YEAR(birthdate) FROM clients LIMIT 10;
```

- Listar nombre y edad de los 10 primeros clientes
```sql
SELECT name, YEAR(NOW()) - YEAR(birthdate) FROM clients LIMIT 10;
```

- Listar clientes que coincidan con el nombre de "Saave"
```sql
SELECT * FROM clients WHERE name LIKE'%Saave%';
```

- Listar clientes (nombre, email, edad y género). con filtro de genero = F y nombre que coincida con 'Lop'.
Usando alias para nombrar la función como 'edad'
```sql
SELECT name, email, YEAR(NOW()) - YEAR(birthdate) AS edad, gender FROM clients WHERE gender = 'F' AND name LIKE '%Lop%';
```

- Listar con dos filtros
```sql
SELECT * FROM authors WHERE author_id > 0 and author_id <= 5;
```

o

```sql
SELECT book_id, author_id, title FROM books WHERE author_id BETWEEN 1 and 5;
```

## Comando JOIN

Los JOIN es tomar dos tablas relacionarlas y desplegar esa informacion ya relacionada.

- Listar todos los autores con ID entre 1 y 5 con los filtro mayor y menor igual
```sql
SELECT * FROM authors WHERE author_id > 0 AND author_id <= 5;
```

- Listar todos los autores con ID entre 1 y 5 con el filtro BETWEEN
```sql
SELECT * FROM authors WHERE author_id BETWEEN 1 AND 5;
```

- Listar los libros con filtro de author_id entre 1 y 5
```sql
SELECT book_id, author_id, title FROM books WHERE author_id BETWEEN 1 AND 5;
```

- Listar nombre y titulo de libros mediante el JOIN de las tablas books y authors
```sql
SELECT b.book_id, a.name, a.author_id, b.title
FROM books AS b
JOIN authors AS a
  ON a.author_id = b.author_id
WHERE a.author_id BETWEEN 1 AND 5;
```

- Listar transactions con detalle de nombre, titulo y tipo. 
Con los filtro genero = F y tipo = Vendido.

- Haciendo join entre transactions, books y clients.
```sql
SELECT c.name, b.title, t.type
FROM transactions AS t
JOIN books AS b
  ON t.book_id = b.book_id
JOIN clients AS c
  ON t.client_id = c.client_id
WHERE c.gender = 'F'
  AND t.type = 'sell';
```

- Listar transactions con detalle de nombre, titulo, autoor y tipo. Con los filtro genero = M y de tipo = Vendido y Devuelto.
- Haciendo join entre transactions, books, clients y authors.
```sql
SELECT c.name, b.title, a.name, t.type
FROM transactions AS t
JOIN books AS b
  ON t.book_id = b.book_id
JOIN clients AS c
  ON t.client_id = c.client_id
JOINauthors AS a
  ON b.author_id = a.author_id
WHERE c.gender = 'M'
  AND t.type IN ('sell', 'lend');
```

### INNER JOIN

Devuelve todas las filas cuando hay al menos una coincidencia en ambas tablas.

- Sin JOIN y con JOIN

Hay una gran diferencia entre el **JOIN implicito ** Y es el RENDIMIENTO sobre la ejecución de la consulta. Esto solo se evidencia al manejar consultas con multiples tablas y dichas tablas una cantidad importante de registros, ademas que es mas claro en la lectura del codigo.

Por lo anterior es mejor siempre el INNER JOIN

```sql
SELECT b.title, a.name 
FROM authors AS a, books AS b 
WHERE a.author_id = b.author_id 
LIMIT 10;
```
```sql
SELECT b.title, a.name 
FROM books AS b 
INNER JOIN authors AS a ON a.author_id = b.author_id 
LIMIT 10;
```
### LEFT JOIN
Devuelve todas las filas de la tabla de la izquierda, y las filas coincidentes de la tabla de la derecha.

```sql
SELECT a.author_id, a.name, a.nationality, b.title FROM authors AS a JOIN books AS b ON b.author_id = a.author_id WHERE a.author_id BETWEEN 1 and 5 ORDER BY a.author_id DESC;
```
- LEFT JOIN para traer datos incluso que no existen, como el caso del author_id = 4 que no tene ningún libro registrado.

```sql
SELECT a.author_id, a.name, a.nationality, b.title FROM authors AS a LEFT JOIN books AS b ON b.author_id = a.author_id WHERE a.author_id BETWEEN 1 and 5 ORDER BY a.author_id ;
```

- Contar número de libros tiene un autor. Con COUNT (contar), es necesario tener un GROUP BY (agrupado por un criterio)
```sql
SELECT a.author_id, a.name, a.nationality, COUNT(b.book_id)
FROM authors AS a
LEFT JOIN books AS b
  ON b.author_id = a.author_id
WHERE a.author_id BETWEEN 1 AND 5
GROUP BY a.author_id
ORDER BY a.author_id;
```

### TIPS DE JOIN

Existen diferentes formas en las que se pueden unir las tablas en nuestras consultas y de acuerdo con esta unión se va a mostrar información, y es importante siempre tener clara esta relación. En esta clase te voy a mostrar gráficamente 7 diferentes tipos de uniones que puedes realizar.

Usar correctamente estas uniones puede reducir el tiempo de ejecución de tus consultas y mejorar el rendimiento de tus aplicaciones.

Como yo lo veo cuando hacemos uniones en las consultas para seleccionar información, estamos trabajando con tablas, estas tablas podemos verlas como conjuntos de información, de forma que podemos asimilar los joins entre tablas como uniones e intersecciones entre conjuntos.

Supongamos que contamos con dos conjuntos, el conjunto A y el conjunto B, o, la tabla A y la tabla B. Sobre estos conjuntos veamos cuál es el resultado si aplicamos diferentes tipos de join.

1. Inner Join

Esta es la forma mas fácil de seleccionar información de diferentes tablas, es tal vez la que mas usas a diario en tu trabajo con bases de datos. Esta union retorna todas las filas de la tabla A que coinciden en la tabla B. Es decir aquellas que están en la tabla A Y en la tabla B, si lo vemos en conjuntos la intersección entre la tabla A y la B.

Esto lo podemos implementar de esta forma cuando estemos escribiendo las consultas:
```sql
SELECT <columna_1> , <columna_2>,  <columna_3> ... <columna_n> 
FROM Tabla_A A
INNER JOIN Tabla_B B
ON A.pk = B.pk
```

2. Left Join

Esta consulta retorna todas las filas que están en la tabla A y ademas si hay coincidencias de filas en la tabla B también va a traer esas filas.

Esto lo podemos implementar de esta forma cuando estemos escribiendo las consultas:
```sql
SELECT <columna_1> , <columna_2>,  <columna_3> ... <columna_n> 
FROM Tabla_A A
LEFT JOIN Tabla_B B
ON A.pk = B.pk
```

3. Right Join

Esta consulta retorna todas las filas de la tabla B y ademas si hay filas en la tabla A que coinciden también va a traer estas filas de la tabla A.

Esto lo podemos implementar de esta forma cuando estemos escribiendo las consultas:
```sql
SELECT <columna_1> , <columna_2>,  <columna_3> ... <columna_n>
FROM Tabla_A A
RIGHT JOIN Tabla_B B
ON A.pk = B.pk
```

4. Outer Join

Este join retorna TODAS las filas de las dos tablas. Hace la union entre las filas que coinciden entre la tabla A y la tabla B.

Esto lo podemos implementar de esta forma cuando estemos escribiendo las consultas:
```sql
SELECT <columna_1> , <columna_2>,  <columna_3> ... <columna_n>
FROM Tabla_A A
FULL OUTER JOIN Tabla_B B
ON A.pk = B.pk
```

5. Left excluding join

Esta consulta retorna todas las filas de la tabla de la izquierda, es decir la tabla A que no tienen ninguna coincidencia con la tabla de la derecha, es decir la tabla B.

Esto lo podemos implementar de esta forma cuando estemos escribiendo las consultas:
```sql
SELECT <columna_1> , <columna_2>,  <columna_3> ... <columna_n>
FROM Tabla_A A
LEFT JOIN Tabla_B B
ON A.pk = B.pk
WHERE B.pk IS NULL
```

6. Right Excluding join

Esta consulta retorna todas las filas de la tabla de la derecha, es decir la tabla B que no tienen coincidencias en la tabla de la izquierda, es decir la tabla A.

Esto lo podemos implementar de esta forma cuando estemos escribiendo las consultas:
```sql
SELECT <columna_1> , <columna_2>,  <columna_3> ... <columna_n>
FROM Tabla_A A
RIGHT JOIN Tabla_B B
ON A.pk = B.pk
WHERE A.pk IS NULL
```

7. Outer excluding join

Esta consulta retorna todas las filas de la tabla de la izquierda, tabla A, y todas las filas de la tabla de la derecha, tabla B que no coinciden.

Esto lo podemos implementar de esta forma cuando estemos escribiendo las consultas:
```sql
SELECT <select_list>
FROM Table_A A
FULL OUTER JOIN Table_B B
ON A.Key = B.Key
WHERE A.Key IS NULL OR B.Key IS NULL
```

### EJEMPLOS DE QUERYS

- ¿Qué nacionalidades hay?
```sql
SELECT nationality FROM authors GROUP BY nationality;
```
- ¿Cuantos escritores hay de cada nacionalidad?
```sql
SELECT nationality, COUNT(author_id) as qty_authors FROM authors GROUP BY nationality;
```
- ¿Cuantos libros hay de cada nacionalidad?
```sql
SELECT a.nationality, COUNT(b.book_id) as qty_book
FROM books as b
JOIN authors as a
ON b.author_id = a.author_id
GROUP BY a.nationality;
```
- ¿Cual es el promedio/desviacion standard del precio de los libros?
```sql
SELECT AVG(price) as avg_price, STDDEV(price) as stddev_price FROM books;
```
- ¿Cual es el precio maximo/minimo de un libro?
```sql
SELECT MAX(price) as max_price, MIN(price) as min_price FROM books;
```
- ¿Cual es el precio maximo/minimo de un libro?
```sql
SELECT c.name, t.type, b.title, a.name, a.nationality
FROM transactions as t
LEFT JOIN clients as c
ON c.client_id = t.client_id
LEFT JOIN books as b
ON b.book_id = t.book_id
LEFT JOIN authorsas a
ON b.author_id = a.author_id;
```

- ¿Cuál es el promedio/desviación standard del precio de libros?
```sql
SELECT a.nationality,  
  AVG(b.price) AS promedio, 
  STDDEV(b.price) AS std 
FROM books AS b
JOIN authorsAS a
  ON a.author_id = b.author_id
GROUP BY a.nationality
ORDER BY promedio DESC;
```

- ¿Cuál es el promedio/desviación standard del precio de libros por nacionalidad?
-- Agrupar por la columna pivot
```sql
SELECT a.nationality,
  COUNT(b.book_id) AS libros,  
  AVG(b.price) AS promedio, 
  STDDEV(b.price) AS std 
FROM books AS b
JOINauthors AS a
  ON a.author_id = b.author_id
GROUP BY a.nationality
ORDER BY libros DESC;
```

- ¿Cuál es el precio máximo/mínimo de un libro?
```sql
SELECT nationality, MAX(price), MIN(price)
FROM books AS b
JOIN authors AS a
  ON a.author_id = b.author_id
GROUP BY nationality;
```

- ¿cómo quedaría el reporte de préstamos?
 CONCAT: para concatenar en cadenas de texto.
TO_DAYS: recibe un timestamp ó un datetime
```sql
SELECT c.name, t.type, b.title, 
  CONCAT(a.name, " (", a.nationality, ")") AS autor,
  TO_DAYS(NOW()) - TO_DAYS(t.created_at)
FROM transactions AS t
LEFT JOIN clients AS c
  ON c.client_id = t.client_id
LEFT JOIN books AS b
  ON b.book_id = t.book_id
LEFT JOIN authors AS a
  ON b.author_id = a.author_id;
```

## UPDATE Y DELETE

La recomendacion antes de hacer alguna modificacion es siempre usar un **SELECT** para ver los campos que quieres modificar.

Y nunca te olvides de **WHERE** en este tipo de comandos

### UPDATE

Como no siempre borrar un dato es la mejor opcion, es recomendable al momento de crear una tabla es tener siempre un booleano con la opcion de **active**.

Este es el comando basico de un update:
```sql
UPDATE <tabla>
SET <columna = valor, ...>
WHERE <condiciones>
LIMIT 1;
```
EJEMPLO:
```sql
UPDATE clients SET active = 0 WHERE client_id = 80 LIMIT 1;
```

- Hacer update a varios
```sql
SELECT client_id, name, active FROM clients WHERE client_id IN (1,6,8,27,90) OR name LIKE '%Lopez%';
```

```sql
UPDATE clients SET active = 0 WHERE client_id IN (1,6,8,27,90) OR name LIKE '%Lopez%'; 

### DELETE

- Borrar un autor en especifico

Es recomendable el poner un **LIMIT** por si llegase a ocurrir un inconveniente, por ejemplo, una condicion que no se pone, que este delete no vaya a borrar de mas informacion.
```sql
DELETE FROM authors WHERE author_id = 161 LIMIT = 1;
```

- Borrar el contenido de la tabla dejando la estructura
```sql
SELECT * FROM transactions;

TRUNCATE transactions;
```
## SUPER QUERYS

- Lo que va ahcer el **COUNT** es sumar 1 cada vez que pasa por un renglon que cumpla los requisitos que le pedimos.
```sql
SELECT count(book_id) FROM books;
```

- El **SUM** va  a operar cada vez que el query vaya avanzando uno por uno dentro de las tuplas. En este caso saldra lo mismo en las dos columnas.
```sql
SELECT count(book_id), sum(1) from books;
```

- Sumar el precio de los libros que tengo por vender.
```sql
SELECT SUM(price) FROM books WHERE sellable = 1;
```

- El numero de copias por el valor de cada una.
```sql
SELECT SUM(price*copies) FROM books WHERE sellable = 1;
```

- Valor de libros que no estan a la venta y  que si estan a la venta.
```sql
SELECT sellable, SUM(prince*copies) FROM books GROUP BY sellable;
```
- Cuantos libros hay antes de cierta fecha.
```sql
SELECT COUNT(book_id), SUM(IF(YEAR < 1950, 1, 0)) AS `<1950` FROM books;
```
```sql
SELECT COUNT(book_id) FROM books WHERE YEAR < 1950;
```

- Cuantos libros hay antes de cierta fecha y cuantos posteriores a esa fecha.
```sql
SELECT COUNT(book_id), SUM(IF(YEAR < 1950, 1, 0)) AS `<1950`, SUM(IF(YEAR < 1950, 0, 1)) AS `>1950` FROM books;
```
```sql
SELECT nationality, COUNT(book_id), 
SUM(IF(year < 1950, 1, 0)) AS "<1950", 
SUM(IF(year >= 1950 and year < 1990, 1, 0)) AS "<1990", 
SUM(IF(year >= 1990 and year < 2000, 1, 0)) AS "<2000",
SUM(IF(year >= 2000, 1, 0)) AS "<hoy"
FROM books;
```

- Cuantos libros hay antes de cierta fecha y cuantos posteriores a esa fecha, por cada pais.
```sql
SELECT nationality, COUNT(book_id), 
SUM(IF(year < 1950, 1, 0)) AS "<1950" 
SUM(IF(year >= 1950 and year < 1990, 1, 0)) AS "<1990" 
SUM(IF(year >= 1990 and year < 2000, 1, 0)) AS "<2000"
SUM(IF(year >= 2000, 1, 0)) AS "<hoy"
FROM books AS b
JOIN authors AS a
ON a.author_id = b.author_id
WHERE a.nationality IS NOT NULL
GROUP BY nationality;
```