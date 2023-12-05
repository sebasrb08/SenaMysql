# Proyecto Sena

## Modelo Entidad Relacion

![image](https://github.com/sebasrb08/SenaMysql/assets/118924181/b3750d3c-7ad6-4ce1-bccd-44d22246a8e0)


## Modelo Relacional

![image](https://github.com/sebasrb08/SenaMysql/assets/118924181/684da8f2-190d-4793-91b5-48819e15923d)



## Creacion de base de  datos
````
CREATE DATABASE sena;
````
## Ingreso a la base  de datos
````
USE sena;
````
##  Creacion de tablas

````
CREATE Table carrera(
    id int not null PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(50)
);

CREATE Table ruta(
    id int not null PRIMARY KEY AUTO_INCREMENT,
    enfasis VARCHAR(50),
    id_carrera int,
    FOREIGN KEY (id_carrera) REFERENCES carrera(id)
);

CREATE Table curso(
    id int not null PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(50) 
    
);

CREATE TABLE matricula(
    id int not null PRIMARY KEY AUTO_INCREMENT
);

CREATE Table aprendiz(
    id int not null PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(50) ,
    id_ruta int,
    FOREIGN KEY (id_ruta) REFERENCES ruta(id)
);


CREATE Table curso_ruta(
    id int not null PRIMARY KEY AUTO_INCREMENT,
    id_curso int,
    id_ruta int,
    FOREIGN KEY (id_curso) REFERENCES curso(id),
    FOREIGN KEY (id_ruta) REFERENCES ruta(id)
);

CREATE Table especialidad(
    id int not null PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(50)
);

CREATE Table instructor(
    id int not null PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(50),
    id_especialidad int,
    FOREIGN KEY (id_especialidad) REFERENCES especialidad(id)
);

CREATE Table instructor_curso(
    id int not null PRIMARY KEY AUTO_INCREMENT,
    id_curso int,
    id_instructor int,
    FOREIGN KEY (id_curso) REFERENCES curso(id),
    FOREIGN KEY (id_instructor) REFERENCES instructor(id)
);


````

## Insertar datos
````
INSERT INTO carrera(nombre)
VALUES("Desarrollo de Software"),("Electrónica"),("Mecánica Automotriz"),
("Seguridad y Salud Ocupacional"),("Soldadura");

INSERT INTO especialidad(nombre)
VALUES("Sistemas"),("Salud Ocupacional"),("Soldadura"),("Mecánica"),("Inglés"),("Electrónica");

INSERT INTO Instructor(nombre,id_especialidad)
VALUES("Ricardo Vicente Jaimes",1),("Marinela Narvaez",2),("Agustín Parra Granados",3),
("Nelson Raúl Buitrago",4),("Roy Hernando Llamas",5),("Maria Jimena Monsalve",6),
("Juan Carlos Cobos",6),("Pedro Nell Santamaría",1),("Andrea González",1),("Marisela Sevilla",2);

INSERT INTO curso(nombre)
VALUES("Matemáticas Básicas"),("Álgebra Computacional"),("Programación Básica"),
("Inglés"),("Electrónica Básica"),("Motor de Cuatro Tiempos"),
("Enfermedades Laborales"),("Higiene Postural en el Trabajo"),("Ergonomía"),
("Legislación Laboral en Colombia"),("Materiales de Soldadura"),("Métodos de Soldadura"),
("Fusión de Metales"),("Buceo 1"),("Buceo 2"),("Riesgo Eléctrico"),
("Bases de Datos Relacionales"),("Bases de Datos NO Relacionales"),("Electrónica Digital"),
("Microprocesadores");


INSERT INTO ruta(enfasis,id_carrera)
VALUES("Sistemas de Información Empresariales",1),("Videojuegos",1),("Machine Learning",1),
("Microcontroladores",2),("Robótica",2),("Dispositivos Bio-médicos",NULL),("Motores Híbridos",NULL),
("Vehículos de Uso Agrícola",NULL),("Sistemas de Gestión en Seguridad Ocupacional",NULL),("Soldadura Autógena Industrial",5),
("Soldadura Eléctrica Industrial",5),("Soldadura Submarina",NULL);


INSERT INTO Aprendiz(nombre,id_ruta)
VALUES("Carlos Saúl Gómez",1),("Leyla María Delgado Vargas",1),("Juan José Cardona",2),
("Sergio Augusto Contreras Navas",2),("Ana María Velázquez Parra",3),("Gustavo Noriega Alzate",4),
("Pedro Nell Gómez Díaz",4),("Jairo Augusto Castro Camargo",5),("Henry Soler Vega",5),
("Antonio Cañate Cortés",11),("Daniel Simancas Junior",10);


INSERT INTO curso_ruta(id_ruta,id_curso)
VALUES(1,17),(1,2),(1,3),(1,18),(1,1),(1,4),(2,1),(2,2),(2,3),
(2,4),(3,3),(3,4),(3,17),(4,5),(4,19),(4,20),(5,5),(5,19),
(5,20),(11,11),(11,13),(10,11),(10,14);

INSERT INTO instructor_curso(id_curso,id_instructor)
VALUES (17,2),(2,1),(3,3),(18,2),(1,4),(4,5),(5,7),
(19,6),(20,7),(11,3),(13,3),(14,NULL),(15,NULL),(16,NULL),
(12,NULL),(10,NULL),(9,NULL),(8,NULL),(7,NULL),(6,NULL)

````
## Parte 2

 1. Agregue un campo Estado_Matrícula a la tabla Matrícula que indique si el estudiante se encuentra “En Ejecución”, “Terminado” o “Cancelado”
 
````
ALTER TABLE matricula ADD COLUMN estado_matricula ENUM("En Ejecución","Cancelado","Terminado");
INSERT INTO matricula (estado_matricula) VALUES("En Ejecución"),("Cancelado"),("Terminado")

ALTER TABLE aprendiz ADD COLUMN id_matricula int;

ALTER TABLE aprendiz ADD CONSTRAINT fk_id_matricula FOREIGN KEY (id_matricula)
REFERENCES matricula(id)
ON DELETE CASCADE
ON UPDATE CASCADE

UPDATE aprendiz
SET id_matricula = 1
WHERE id in (1,2,4,5);

UPDATE aprendiz
SET id_matricula = 2
WHERE id in (3,9);

UPDATE aprendiz
SET id_matricula = 3
WHERE id in (6,7,8,10,11);
````

2 . Agregue a el campo edad a la tabla de Aprendices.

````
ALTER TABLE aprendiz ADD COLUMN edad int;
````

3 . Si suponemos que los cursos tienen una duración diferente
dependiendo de la ruta que lo contenga ¿qué modificación haría a la
estructura de datos ya planteada?

````
ALTER TABLE curso_ruta ADD COLUMN duracion VARCHAR(50);
````

4 . Seleccionar los nombres y edades de aprendices que están cursando la
carrera de electrónica.

````
SELECT aprendiz.nombre,edad,carrera.nombre FROM aprendiz 
INNER JOIN ruta
ON aprendiz.id_ruta = ruta.id
INNER JOIN carrera
ON ruta.id_carrera = carrera.id
WHERE carrera.nombre = "Electrónica"
````
Resultado:

+------------------------------+------+-------------+
| nombre                       | edad | nombre      |
+------------------------------+------+-------------+
| Gustavo Noriega Alzate       | NULL | Electrónica |
| Pedro Nell Gómez Díaz        | NULL | Electrónica |
| Jairo Augusto Castro Camargo | NULL | Electrónica |
| Henry Soler Vega             | NULL | Electrónica |
+------------------------------+------+-------------+

5 . Seleccionar Nombres de Aprendices junto al nombre de la ruta de
aprendizaje que cancelaron.

````
SELECT aprendiz.nombre ,ruta.enfasis
FROM aprendiz
INNER JOIN ruta
ON aprendiz.id_ruta = ruta.id
INNER JOIN matricula
ON aprendiz.id_matricula = matricula.id
WHERE matricula.estado_matricula = "Cancelado"
````

6 . Seleccionar Nombre de los cursos que no tienen un instructor asignado.

````
SELECT curso.nombre 
FROM instructor_curso as ic
INNER JOIN curso
ON ic.id_curso = curso.id
WHERE ic.id_instructor IS NULL;
````

7 . Seleccionar Nombres de los instructores que dictan cursos en la ruta de
aprendizaje “Sistemas de Información Empresariales”.
````
SELECT DISTINCT i.nombre
FROM instructor as i
INNER JOIN instructor_curso as ic
ON i.id = ic.id_instructor
INNER JOIN curso_ruta as cr
ON ic.id_curso = cr.id_curso
INNER JOIN ruta
ON cr.id_ruta = ruta.id
WHERE ruta.enfasis = "Sistemas de Información Empresariales"
````

8 . Genere un listado de todos los aprendices que terminaron una Carrera
mostrando el nombre del profesional, el nombre de la carrera y el
énfasis de la carrera (Nombre de la Ruta de aprendizaje)

````
SELECT ap.nombre
FROM aprendiz as ap
INNER JOIN ruta as r
ON ap.id_ruta =r.id 
INNER JOIN carrera as ca
ON r.id_carrera = ca.id
INNER JOIN matricula as ma
ON ap.id_matricula = ma.id
WHERE ma.estado_matricula = "Terminado"
````

10 . Nombres de Instructores que no tienen curso asignado.

````
SELECT nombre
FROM instructor
WHERE id NOT IN (SELECT DISTINCT id_instructor FROM instructor_curso WHERE id_instructor IS NOT NULL);
````
