
## Introducción
 En apache se refiere a la implementación de medidas de seguridad adicionales con el objetivo de reducir la superficie de ataque y fortalecer la configuración del servidor Apache. Apache es uno de los servidores web más utilizados, y endurecer su configuración es esencial para mitigar riesgos de seguridad.
 Con los siguientes pasos podremos implementarlo en nuestra propia página web.

 ## Desarrollo
 1. Cómo Ocultar la Versión de Apache e Información del Sistema Operativo:
    Abre el archivo de configuración principal de Apache, que generalmente se encuentra en /etc/apache2/apache2.conf
```bash
$sudo nano /etc/apache2/apache2.conf
```
    Modifica las siguientes líneas en el archivo de configuración para desactivar la exposición de la versión de Apache y del sistema operativo
```bash
 ServerSignature Off
 ServerTokens Prod
```
   La línea ServerSignature Off oculta la información del servidor en las páginas de error, y ServerTokens Prod establece que solo se muestre la información     c    esencial del servidor.

2. Configuración de Seguridad del Directorio:
   Configura la seguridad del directorio y la configuración del propio host. Limita el acceso y desactiva la enumeración de directorios del archivo "/var/www         /html":
```bash
<Directory "/var/www/html">
    Options -Indexes
    AllowOverride All
    Order allow,deny
    Allow from all
</Directory>
```

4. Configuración de SSL/TLS:
   Edita el archivo de configuración de Apache2(/etc/apache2/apache2.conf) para incluir configuraciones seguras de SSL/TLS. Ajusta las directivas SSLCipherSuite y    SSLProtocol:
```bash
SSLProtocol -ALL +TLSv1.2
SSLCipherSuite EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH
```

6. Limitar Métodos HTTP:
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

8. desactivar la directiva ServerSignature en Apache
   Abre el archivo de configuración principal de Apache(/etc/apache2/apache2.conf)
   Agrega la Directiva ServerSignature. Busca una línea que contenga la directiva ServerSignature. Si no existe, puedes agregarla. Establécela en Off
```bash
>ServerSignature Off
```
9.establecer la directiva ServerTokens en 'Prod' en Apache
  Edita el Archivo de Configuración de Apache
  Busca una línea que contenga la directiva ServerTokens. Si no existe, puedes agregarla. Establécela en 'Prod'
```bash
ServerTokens Prod
```
11.Identifica los Módulos Innecesarios:
   Lista los módulos actualmente cargados en tu servidor Apache para identificar cuáles consideras innecesarios. Puedes utilizar el siguiente comando para listar     los módulos habilitados
```bash
$apache2ctl -M  # Para Ubuntu/Debian
```
   Elimina las Directivas de Carga de Módulos.Dentro del archivo de configuración, busca las líneas que cargan los módulos innecesarios y coméntalas o elimina las    líneas. Las directivas de carga de módulos suelen tener un formato como:
```bash
LoadModule module_name_module modules/mod_module_name.so
```
14.limitar el tamaño de carga de archivos en Apache
   Edita la Configuración de Apache
   grega las siguientes líneas al archivo de configuración para establecer límites en el tamaño de carga de archivos. Puedes ajustar el tamaño según tus necesidades. Por ejemplo, para limitar el tamaño de carga a 10 megabytes
```bash
LimitRequestBody 10485760  # 10 megabytes en bytes
```
16.Correr Apache como un usuario y grupo separado 
   Crear un usuario y grupo dedicados:
```bash
sudo groupadd micomandogrupo
sudo useradd -M -s /sbin/nologin -g micomandogrupo micomandousuario
```
   Cambiar la propiedad de los archivos de Apache. Cambie la propiedad de los archivos y directorios de Apache al usuario y grupo recién creados.
```bash
sudo chown -R micomandousuario:micomandogrupo /ruta/a/apache
```
   actualizar la configuración de Apache. Edite su archivo de configuración de Apache (generalmente ubicado en /etc/httpd/httpd.conf
```bash
User micomandousuario
Group micomandogrupo
```
17.Protegerse contra los ataques de denegación de servicio distribuido (DDoS) 
    Aquí tienes algunas estrategias generales:
    
    Usar una Red de Entrega de Contenido (CDN):
        Distribuir el contenido en varios servidores en todo el mundo para reducir el impacto de los ataques DDoS al dispersar la carga geográficamente.

    Implementar Límites de Tasa:
        Establecer límites de tasa para restringir la cantidad de solicitudes que un usuario puede realizar en un período de tiempo específico. Esto puede ayudar         a mitigar los ataques DDoS al limitar la velocidad a la que se procesan las solicitudes.

    Desplegar un Cortafuegos de Aplicaciones Web (WAF):
        Un WAF puede filtrar y supervisar el tráfico HTTP entre una aplicación web e Internet. Puede ayudar a bloquear patrones comunes de ataque y proporcionar          protección contra varios tipos de ataques DDoS.

    Usar IP Anycast:
        La IP Anycast dirige el tráfico al servidor más cercano, distribuyendo la carga y dificultando que los atacantes concentren el tráfico en un único                objetivo.   

    Monitorear Patrones de Tráfico:
        Monitorea regularmente el tráfico de tu red para identificar patrones inusuales que puedan indicar un ataque DDoS. Implementa alertas para actividades            sospechosas.
