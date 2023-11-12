
## Introducción
 En apache se refiere a la implementación de medidas de seguridad adicionales con el objetivo de reducir la superficie de ataque y fortalecer la configuración del servidor Apache. Apache es uno de los servidores web más utilizados, y endurecer su configuración es esencial para mitigar riesgos de seguridad.
 Con los siguientes pasos podremos implementarlo en nuestra propia página web.

 ## Desarrollo
 1. Modificar los siguientes archivos de configuración de apache para que no se puedan ver archivos ocultos:
```bash
$sudo nano/etc/apache2/apache2.conf

    <Directory /var/www/html/>
            Options -Indexes 
    </Directory>
```
