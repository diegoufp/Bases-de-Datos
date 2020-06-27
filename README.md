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



## Relaciones 

Las **relaciones** nos permiten ligar o unir nuestras diferentes entidades y se representan con rombos. Por convención se definen a través de verbos.

Las relaciones tienen una propiedad llamada **cardinalidad** y tiene que ver con números. Cuántos de un lado pertenecen a cuántos del otro lado:


- Cardinalidad: 1 a 1
- Cardinalidad: 0 a 1
- Cardinalidad: 1 a N
- Cardinalidad: 0 a N
- Cardinalidad: N a N
