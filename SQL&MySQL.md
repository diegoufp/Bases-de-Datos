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

```
CREATE database blogpost;
```

- Crear la tabla si aun no existe:

```
CREATE DATABASE IF NOT EXISTS blogpost;
```

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

- **UNSIGNED**: solo aceptará enteros positivos.

- **TINYINT**: Ocupa 1 byte.

- **SMALLINT**: Ocupa 2 byes.

- **Varchar**: Tu le tienes que indicar el numero de caracteres a almacenar de texto.EJEMPLO VARCHAR(2) SI.

- **NOTNULL**: Es decirle a MYSQL que no almacene algo ,algo nulo es que no existe información, en otras palabras es nada.

- **DEFAULT**: Es que no le mando información por defecto debe poner el valor que se agrega después del mismo.

- **COMMENT**: Comentario a la columna, que solo se ve al revisar el código.

- **DOUBLE**: Almacena los números enteros y los decimales. *DOUBLE (6,2) esto hace que de los 6 números que le estamos diciendo 2 de ellos los use solo para decimales. 

- **FLOAT**: Es un numero que esta almacenando hasta 6 decimales, es para cálculos precisos.

- **TEXT**: Es meter tanto como se pueda. EJEMPLO Descripción de un libro


```
CREATE TABLE `books` (
  `book_id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `author_id` int(10) unsigned DEFAULT NULL,
  `title` varchar(100) NOT NULL,
  `year` int(11) NOT NULL DEFAULT '1900',
  `language` varchar(2) NOT NULL COMMENT 'ISO 639-1 Language code (2 chars)',
  `cover_url` varchar(500) DEFAULT NULL,
  `price` double(6,2) DEFAULT NULL,
  `sellable` tinyint(1) NOT NULL DEFAULT '0',
  `copies` int(11) NOT NULL DEFAULT '1',
  `description` text,
  PRIMARY KEY (`book_id`),
  UNIQUE KEY `book_language` (`title`,`language`)
) ENGINE=InnoDB AUTO_INCREMENT=199 DEFAULT CHARSET=utf8;
```

