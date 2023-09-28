## Introducción
En la actividad deseamos dentro del archivo Prueba tenga permisos necesarios para los grupos "teachers, students, eso1 y eso2".

## Paso1
Creamos los grupos que necesitamos con el comando "addgroup" y los usuarios con "adduser".
Los usuarios s1 y s2 van con el grupo eso1, eso2; los usuarios t1 y t2 con los teachers y s3 con estudents.
## Paso2
Después creamos las carpetas con el comando mkdir "teachers, students, eso1 y eso2" donde se guardaran los archivos respectivamente de cada grupo.

![Alt text](permisos_Carlos-1.png)

## Paso3
Con el comando "setfacl -Rdm g:grupo:permisos" donde teachers tendran permisos de lectura y escritura en todos las carpetas y los demás tendran permidos de lectura con sus grupos correspondientes.
