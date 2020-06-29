# Fundamentos en Bases de Datos



## Tipos de bases de datos

- Relacionales : En la industria hay varias compañias dedicadas a ser manejadoras de bases de datos relacionales como [SQL Server](https://www.microsoft.com/es-mx/sql-server/sql-server-downloads "SQL Server"), [Oracle](https://www.oracle.com/es/index.html "Oracle"), [MariaDB](https://mariadb.com/?_ga=2.123419019.1633320114.1593138543-55489294.1593138543&_gac=1.119303547.1593138543.CjwKCAjwltH3BRB6EiwAhj0IUP-B0C8Tw8ZyzSJPtz3kG2Y8XXOGdPRTF3R1GKzm-eS73XeOmvuKMxoCa5EQAvD_BwE "MariaDB"), entre otras.

- No relacionales: Todavía están avanzando y existen ejemplos muy distintos como [cassandra](https://cassandra.apache.org/ "cassandra"), [elasticsearch](https://www.elastic.co/es/ "elasticsearch"), [neo4j](https://neo4j.com/ "neo4j"), [MongoDB](https://www.mongodb.com/es "MongoDB"), entre otras.


##### Servicios:
- Auto administados: Es la base de datos que instalas tú y te encargas de actualizaciones, mantenimiento, etc.
- Administrados: Servicios que ofrecen las nubes modernas como [Azure](https://azure.microsoft.com/es-mx/ "Azure") y no debes preocuparte por mantenimiento o actualizaciones.



## Historia de las RDB (bases de datos relacionales)

Las bases de datos surgen de la necesidad de conservar la información más allá de lo que existe en la memoria RAM.

Las bases de datos **basadas en archivos** eran datos guardados en texto plano, fáciles de guardar pero muy difíciles de consultar y por la necesidad de mejorar esto nacen las **bases de datos relacionales**. Su inventor **Edgar Codd** dejó ciertas reglas para asegurarse de que toda la filosofía de las bases de datos no se perdiera, estandarizando el proceso.

- **Regla 0 (fundación)**:
Cualquier sistema que se proclame como relacional, debe ser capaz de gestionar sus bases de datos enteramente mediante sus capacidades relacionales.

- **Regla 1 (información)**:
Toda la información en la base de datos se representa por valores dentro de una tabla relacional que tiene columnas y filas
No puede haber información a la que accedemos por otra via (Fuera de las tablas).

- **Regla 2 (acceso garantizado)**:
Cualquier dato es accesible sabiendo la clave de su fila y el nombre de su columna o atributo.

- **Regla 3 (Tratamiento sistemáticos de los valores nulos)**:
La base de datos debe tener la capacidad de manejar la lectura de datos nulos o en blanco para lograr la representación de datos faltantes o inexistentes.

- **Regla 4 (Catalogo en línea relacional)**:
El sistema debe tener un catálogo en línea (Diccionario de datos) y los usuarios autorizados deben poder ingresar a él.

- **Regla 5 (Sublenguaje de datos completos)**:
Al menos tiene que existir un lenguaje capaz de hacer todas las funciones sobre el sistema de bases de datos.

- **Regla 6 (Vistas actualizadas)**:
El sistema se debe actualizar automáticamente sin la necesidad de hacerlo manualmente
No puede haber diferencia entre los datos de las vistas y los datos de las tablas de base.

- **Regla 7 (Inserciones, modificaciones y eliminaciónes de alto nivel)**:
La base de datos debe tener la capacidad de poder mover, copiar o borrar grandes cantidades de datos a la vez.

- **Regla 8 (Independencia física de datos)**:
Los programas de aplicación y actividades del terminal permanecen inalterados a nivel lógico aunque realicen cambios en las representaciones de almacenamiento o métodos de acceso.

- **Regla 9 (Independencia lógicas de los datos)**:
Los programas de aplicación y actividades del terminal permanecen inalterados a nivel lógico aunque se realicen cambios a las tablas base que preserven la información

- **Regla 10 (independencia de la integridad)**:
Las restricciones de integridad se deben especificar por separado de los programas de aplicación y almacenarse en la base de datos. Debe ser posible cambiar esas restricciones sin afectar innecesariamente a las aplicaciones existentes

- **Regla 11 (Independencia de la distribución)**:
La distribución de porciones de base de datos en distintas localizaciones debe ser invisible a los usuarios de la base de datos.

- **Regla 12 (La regla de la no subversión)**:
Si el sistema proporciona una interfaz de bajo nivel de registro, aparte de una interfaz relacional, esa interfaz de bajo nivel no debe permitir su utilización para subvertir el sistema



## Entidades y atributos

Una **entidad** es algo similar a un objeto ([programación orientada a objetos](https://es.wikipedia.org/wiki/Programaci%C3%B3n_orientada_a_objetos "programación orientada a objetos")) y representa algo en el mundo real, incluso algo abstacto. Tiene atributos que son las cosas que los hacen ser una entidad y por convención se ponen en plural.


### Entidades:

**Entidades fuertes**: Esto quiere decir que no dependen de ninguna otra entidad para existir (se representan con un cuadrado).

**Entidades débiles**: Significa que una entidad debil no puede existir sin una entidad fuerte (se representan con un cuadrado con doble linea).

Las entidades debiles pueden ser debiles por dos mitivos:

- **Débiles por identidad**: No se diferencian entre sí más que por la clave de su identidad fuerte.

- **Débiles por existencia**: Se les asigna una clave propia.


### Atributos:

**Atributos compuestos**: Son aquellos que tienen atributos ellos mismos.

**Atributo especial**: Se puede inferir a partir del año (se representa con un circulo punteado).

**Atributo multivaluado**: Que tiene mas de uno (se representa con un circulo con doble linea).

**Atributos llave**: son aquellos que identifican a la entidad y no pueden ser repetidos(se representa con la palabra subrayada). Existen:
    
- **Naturales**: Son inherentes al objeto como el número de serie

- **Clave artificial**: No es inherente al objeto y se asigna de manera arbitraria.



## Entidades de un Blogpost

Nuestro proyecto será un manejador de Blogpost. Es un contexto familiar y nos representará retos muy interesantes.

- **Primer paso**: Identificar las entidades.
- **Segundo paso**: Pensar en los atributos.

### Normalizando en diagrama fisico

#### Regla general:

Cuando tienes una relacion **uno a uno**, no importa a cual tabla le pongas la referencia de la otra tabla([FOREIGN KEY](https://github.com/diegoufp/Bases-de-Datos#constraints-restricciones "OREIGN KEY")), es indistinto.

Cuando tiene una relacion **uno a muchos** es muy importante que en la tabla donde tienes la terminacion **muchos**, en esa tabla, vas a poner la llave foranea de la tabla que tiene uno.


## Relaciones 

Las **relaciones** nos permiten ligar o unir nuestras diferentes entidades y se representan con rombos. Por convención se definen a través de verbos.

Las relaciones tienen una propiedad llamada **cardinalidad** y tiene que ver con números. Cuántos de un lado pertenecen a cuántos del otro lado:


- **Cardinalidad 1 a 1**: Un registro de una entidad A se relaciona con solo un registro en una entidad B y viceversa.

- **Cardinalidad 0 a 1**: Un registro de una entidad A se relaciona con solo un registro en una entidad B y pero ningún registro de B se relaciona con A.

- **Cardinalidad 1 a N**: Una entidad en A se relaciona con cero o muchas entidades en B. Pero una entidad en B se relaciona con una única entidad en A.

- **Cardinalidad 0 a N**: Una entidad en A se relaciona con cero o muchas entidades en B. Pero ninguna entidad en B se relaciona una entidad en A.

- **Cardinalidad N a N**: Una entidad en A se puede relacionar con 0 o muchas entidades en B y viceversa.



## Diagrama ER ([Entidad Relacion](https://en.wikipedia.org/wiki/Entity%E2%80%93relationship_model#Cardinalities "Entidad Relacion"))

Un diagrama es como un mapa y nos ayuda a entender cuáles son las entidades con las que vamos a trabajar, cuáles son sus relaciones y qué papel van a jugar en las aplicaciones de la base de datos.

Existen varias nomenclaturas para hacer este tipo de diagramas pero esta era la origial y es una de las mas utilizadas.



## Diagrama Físico: tipos de datos y constraints

Para llevar a la práctica un diagrama debemos ir más allá y darle detalle con parámetros como:

### Tipos de dato:

#### Texto:
- **CHAR**: almacenamiento de cadenas de caracteres.
- **VARCHAR**: almacenamiento de cadenas de caracteres con mayor dinamismo, para cuando no conocemos con exactitud el tamaño de las cadenas de texto que queremos introducir. Puede contener un maximo de 255 caracteres.
- **TEXT**: almacenamiento de grandes cadenas de caracteres.

#### Números: 
- **INTEGER**: números sin decimales.
- **BIGINT**: números enteros de gran tamaño.
- **SMALLINT**: números enteros de mínimo tamaño (99 o menos).
- **DECIMAL**: almacena números con parte decimal.
- **NUMERIC**: mismo funcionamiento que DECIMAL.

#### Fecha/hora: 
La fecha y hora en general en las bases de datos, es de los datos mas interesantes, por que internamente nos sirve para saber cuando fue creado un registro, cuando alguien lo modificó, cuando alguien los borró. 

- **DATE**: soporta datos de AÑO/MES/DÍA.
- **TIME**: guarda datos de HORA DEL DÍA.
- **DATETIME y TIMESTAMP**: combina DATE+TIME.

#### Lógicos: 
- **BOOLEAN**: Verdadero o Falso.

### Constraints (Restricciones)

- **NOT NULL**: Se asegura que la columna no tenga valores nulos.
- **UNIQUE**: Se asegura que cada valor en la columna no se repita.
- **PRIMARY KEY**: Es una combinación de NOT NULL y UNIQUE.
- **FOREIGN KEY**: Identifica de manera única una tupla en otra tabla.
- **CHECK**: Se asegura que el valor en la columna cumpla una condición dada.
- **DEFAULT**: Coloca un valor por defecto cuando no hay un valor especificado.
- **INDEX**: permite búsquedas más ágiles. Útil cuando se requieren lecturas frecuentes, sin escritura constante de datos.



## Diagrama Físico: normalización

La [normalización](https://platzi.com/blog/normalizar-una-base-de-datos-y-no-morir-en-el-intento/ "normalización") como su nombre lo indica nos ayuda a dejar todo de una forma normal. Esto obedece a las [12 reglas de Codd](https://github.com/diegoufp/Bases-de-Datos#historia-de-las-rdb-bases-de-datos-relacionales "12 reglas de Codd") y nos permiten separar componentes en la base de datos:


#### Primera forma normal (1FN): 
Esta FN nos ayuda a eliminar los valores repetidos y no atómicos dentro de una base de datos.

Formalmente, una tabla está en primera forma normal si:

- Todos los atributos son atómicos. Un atributo es atómico si los elementos del dominio son simples e indivisibles.
- No debe existir variación en el número de columnas.
- Los campos no clave deben identificarse por la clave (dependencia funcional).
- Debe existir una independencia del orden tanto de las filas como de las columnas; es decir, si los datos cambian de orden no deben cambiar sus significados.

Se traduce básicamente a que si tenemos campos compuestos como por ejemplo “nombre_completo” que en realidad contiene varios datos distintos, en este caso podría ser “nombre”, “apellido_paterno”, “apellido_materno”, etc.

También debemos asegurarnos que las columnas son las mismas para todos los registros, que no haya registros con columnas de más o de menos.

Todos los campos que no se consideran clave deben depender de manera única por el o los campos que si son clave.

Los campos deben ser tales que si reordenamos los registros o reordenamos las columnas, cada dato no pierda el significado.


#### Segunda forma normal (2FN): 
Esta FN nos ayuda a diferenciar los datos en diversas entidades.

Formalmente, una tabla está en segunda forma normal si:

- Está en 1FN
- Sí los atributos que no forman parte de ninguna clave dependen de forma completa de la clave principal. Es decir, que no existen dependencias parciales.
- Todos los atributos que no son clave principal deben depender únicamente de la clave principal.

Lo anterior quiere decir que sí tenemos datos que pertenecen a diversas entidades, cada entidad debe tener un campo clave separado. 

#### Tercera forma normal (3FN): 
Esta FN nos ayuda a separar conceptualmente las entidades que no son dependientes.

Formalmente, una tabla está en tercera forma normal si:

Se encuentra en 2FN
No existe ninguna dependencia funcional transitiva en los atributos que no son clave

Esta FN se traduce en que aquellos datos que no pertenecen a la entidad deben tener una independencia de las demás y debe tener un campo clave propio. 

#### Cuarta forma normal (4FN): 

Esta FN nos trata de atomizar los datos multivaluados de manera que no tengamos datos repetidos entre rows.

Formalmente, una tabla está en cuarta forma normal si:

- Se encuentra en 3FN
- Los campos multivaluados se identifican por una clave única

Esta FN trata de eliminar registros duplicados en una entidad, es decir que cada registro tenga un contenido único y de necesitar repetir la data en los resultados se realiza a través de claves foráneas.

Aplicado al ejemplo anterior la tabla materia se independiza y se relaciona con el alumno a través de una tabla transitiva o pivote, de tal manera que si cambiamos el nombre de la materia solamente hay que cambiarla una vez y se propagara a cualquier referencia que haya de ella.

De esta manera, aunque parezca que la información se multiplicó, en realidad la descompusimos o normalizamos de manera que a un sistema le sea fácil de reconocer y mantener la consistencia de los datos.

Algunos autores precisan una 5FN que hace referencia a que después de realizar esta normalización a través de uniones (JOIN) permita regresar a la data original de la cual partió.



## RDBMS

RDBMS significa Relational Database Management System o sistema manejador de bases de datos relacionales. Es un programa que se encarga de seguir las reglas de Codd y se puede utilizar de manera programática.

Hay dos maneras de acceder a manejadores de bases de datos:

- Instalar en máquina local un administrador de bases relacional.
- Tener ambientes de desarrollo especiales o servicios cloud.

En este curso usaremos [MySQL](https://dev.mysql.com/downloads/ "MySQL") porque tiene un impacto histórico siendo muy utilizado y además es software libre y gratuito. La versión 5.6.43 es compatible con la mayoría de aplicaciones y frameworks.

- Root es el usuario principal que tendrá todos los permisos y por lo tanto en ambientes de producción hay que tener mucho cuidado al configurarlo.

## Usar mysql en KALI LINUX 2020.b

$ `sudo apt-get install mariadb-client`

$ `sudo apt-get install mariadb-server`

$ `sudo service mysql start`

$ `sudo mariadb`

#### Ya en MariaDB

$ MariaDB [(none)]> `show databases`

$ MariaDB [(none)]> `use mysql`

$ MariaDB [(mysql)]> `show tables`

$ MariaDB [(mysql)]>`exit`



## Servicios administrados

Hoy en día muchas empresas ya no tienen instalados en sus servidores los **RDBMS** sino que los contratan a otras personas. Estos servicios administrados cloud te permiten concentrarte en la base de datos y no en su administración y actualización.



## Historia de SQL

**SQL** significa **S**tructured **Q**uery **L**anguage y tiene una estructura clara y fija. Su objetivo es hacer un solo lenguaje para consultar cualquier manejador de bases de datos volviéndose un gran estándar. Es un lenguaje de dominio específico utilizado en programación, diseñado para administrar, y recuperar información de sistemas de gestión de bases de datos relacionales

Ahora existe el **NOSQL** o **N**ot **O**nly **S**tructured **Q**uery **L**anguage que significa que no sólo se utiliza SQLen las bases de datos no relacionales.
Israel Vázquez Morales



## [DDL](https://www.w3schools.in/mysql/ddl-dml-dcl/ "DDL") create

**SQL** tiene dos grandes sublenguajes:
**DDL** o [Data Definition Language](https://en.wikipedia.org/wiki/Data_definition_language "Data Definition Language") que nos ayuda a crear la estructura de una base de datos. Existen 3 grandes comandos:

- **Create** (Crear): Este comando permite crear objetos de datos, como nuevas bases de datos, tablas, vistas, índices, etc.
- **Alter** (Alterar): Este comando permite modificar la estructura de una tabla u objeto.
- **Drop** (Eliminar): Este comando elimina un objeto de la base de datos
- **Truncate** (Truncar): Este comando solo aplica a tablas y su función es borrar el contenido completo de la tabla especificada.

**3 objetos que manipularemos con el lenguaje DDL**:

- Database o bases de datos
- Table o tablas. Son la traducción a SQL de las entidades
- View o vistas: Se ofrece la proyección de los datos de la base de datos de forma entendible.

### Comandos

- **Creas base de datos**:
`CREATE DATABASE test_db;`

- **Usar base de datos**:
`USE DATABASE test_db;`

- **Creas tabla**:
`CREATE TABLE people (person_id int, last_name varchar(255), first_name varchar(255), address varchar(255), city varchar(255));`


### Crear base de datos de ejemplo

`$ sudo mariadb`

`> use mysql`

``> CREATE SCHEMA `blogpost` DEFAULT CHARACTER SET utf8;``

`> USE blogpost;`

``> CREATE TABLE `blogpost`.`people` (`person_id` INT NOT NULL AUTO_INCREMENT, `last_name` VARCHAR(255) NULL, `first_name` VARCHAR(255) NULL, `address` VARCHAR(255) NULL, `city` VARCHAR(255) NULL, PRIMARY KEY (`person_id`));``



##  CREATE VIEW y DDL ALTER

**VIEW** sirve para representar los datos que necesitamos, hacer un consulta especifica y se identifica las vistas con una v al principio del nombre.

- Vista: `CREATE VIEW v_brasil_customers AS`
            `SELECT customer_name, contact_name`
            `FROM customers`
            `WHETE country = "Brasil";`

**ALTER** ES el comando que nos va a permitir modificar.

- Agregar columna:  `ALTER TABLE people`
                    `ADD date_of_birth date:`

- Alterar columna:  `ALTER TABLE people`
                    `ALTER COLUMN date_of_birth year;`

- Borrar columna:   `ALTER TABLE people`
                    `DROP COLUMN date_of_birth;`


### Usar comandos en un ejemplo

`> USE blogpost;`

``> CREATE OR REPLACE VIEW `v_blogpost_people` AS SELECT * FORM blogpost.people;``

``> ALTER TABLE `blogpost`.`people` ADD COLUMN `date_of_birth` DATETIME NULL AFTER `city`;``

``> ALTER TABLE `blogpost`.`people` CHANGE COLUMN `date_of_birth` `date_of_birth` VARCHAR(30) NULL DEFAULT NULL;``

``> ALTER TABLE `blogpost`.`people` DROP COLUMN `date_of_birth`;``