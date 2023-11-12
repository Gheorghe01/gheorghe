
## Introducción
 En apache se refiere a la implementación de medidas de seguridad adicionales con el objetivo de reducir la superficie de ataque y fortalecer la configuración del servidor Apache. Apache es uno de los servidores web más utilizados, y endurecer su configuración es esencial para mitigar riesgos de seguridad.
 Con los siguientes pasos podremos implementarlo en nuestra propia página web.

 ## Desarrollo
 1. Modificar los siguientes archivos de configuración de apache para que no se puedan ver archivos ocultos:
```bash
$sudo nano /etc/apache2/apache2.conf

    <Directory /var/www/html/>
            Options -Indexes 
    </Directory>
```
2. Desactivar Módulos Innecesarios:
Desactiva los módulos que no necesitas. Puedes hacerlo utilizando el comando a2dismod seguido del nombre del módulo:
```bash
sudo a2dismod -f autoindex
sudo systemctl restart apache2
```
3. Configuración de SSL/TLS:
 Edita el archivo de configuración de Apache2(/etc/apache2/apache2.conf) para incluir configuraciones seguras de SSL/TLS. Ajusta las directivas SSLCipherSuite y SSLProtocol:
```bash
SSLProtocol -ALL +TLSv1.2
SSLCipherSuite EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH
```
4. Configuración de Seguridad del Directorio:
Configura la seguridad del directorio y la configuración del propio host. Limita el acceso y desactiva la enumeración de directorios del archivo "/var/www/html":
```bash
<Directory "/var/www/html">
    Options -Indexes
    AllowOverride All
    Order allow,deny
    Allow from all
</Directory>
```
5. Limitar Métodos HTTP:
Limita los métodos HTTP permitidos. Puedes hacerlo utilizando la directiva Limit en tu configuración de apache($sudo nano/etc/apache2/apache2.conf):
```bash
<LimitExcept GET POST>
    Require all denied
</LimitExcept>
```
6. Firewall:
Configura un firewall para limitar el tráfico entrante y saliente. Puedes usar UFW (Uncomplicated Firewall):
```bash
sudo ufw enable
sudo ufw allow 80
sudo ufw allow 443
```
