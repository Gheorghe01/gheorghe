### Política de contraseñas

## Introducción
seguridad de nuestro sistema puede verse amenazada.
El objetivo de la práctica es añadir una capa más para que los usuarios de la red.

Lo haremos con el servicio PAM o módulos de autenticación conectables (Pluggable Authentication Modules)
son la capa de gestión que se encuentran entre las aplicaciones de Linux y el sistema de autenticación nativo del Linux.

Y voy a describiros como cambiar la gestión de contraseñas con pam_pwquality (pam_cracklib).
pwquality es una versión más actual y mejorada de cracklib. pwquality llama a una rutina cracklib para verificar si la contraseña es parte de un diccionario si este se especifica en la directiva dictpath.


 ## instalación:
```bash
$sudo apt install -y libpam-cracklib libpam-pwquality libpwquality-tools
```
 ## configuración: 
Una vez instalado el módulo de libpwquality podemos editar sus opciones en el fichero "/etc/security/pwquality.conf".
Con las siguientes opciones podemos cambiar las políticas de seguridad de la contraseña:


        - difok: Número de caracteres en una nueva contraseña que no deben estar presentes en la contraseña anterior.
        - minlen: Tamaño mínimo aceptable para la nueva contraseña.
        - dcredit: Crédito máximo por tener dígitos en la nueva contraseña.
        - ucredit: Crédito máximo por tener letras mayúsculas en la nueva contraseña.
        - lcredit: Crédito máximo por tener letras minúsculas en la nueva contraseña.
        - ocredit: Crédito máximo por tener otros caracteres en la nueva contraseña.
        - minclass: Número mínimo de clases de caracteres requeridas para la nueva contraseña
        - maxrepeat: Número máximo de caracteres repetidos.
        - maxclassrepeat: Número máximo de caracteres consecutivos en la misma clase.
        - gecoscheck: Verifica si las palabras individuales de más de 3 caracteres del campo passwd GECOS (campo de comentarios) del - usuario están contenidas en la nueva contraseña.
        - dictpath: Ruta a los diccionarios de clacklib.
        - badwords: Lista de palabras separadas por espacios que no deben incluirse en la contraseña.


    * Comprobación
        Con nano he editado el archivo "/etc/pam.d/common-password"
        sudo nano /etc/pam.d/common-password

        Y en la línea ""14,15,17,21"



     



    




