IMPLEMENTACIÓN DE UNA RED EMPRESARIAL



[![Aspose-Words-9f5f01c8-5adf-4172-9b6c-3424ac8ff944-001.png](https://i.postimg.cc/52sqC9Wn/Aspose-Words-9f5f01c8-5adf-4172-9b6c-3424ac8ff944-001.png)](https://postimg.cc/bZ2tXhJt)


Gheroghe Covaci Coldar, Santiago E. Carpente Burguete, Javier Contell Adell Seguridad y Alta Disponibilidad

2º ASIR- 2023/24

ÍNDICE

[**Introducción........................................................................................................................... 3**](#_page2_x72.00_y72.00)


[**Retos y Objetivos...................................................................................................................3**](#_page2_x72.00_y191.73)


  [Implementación de la Arquitectura de Red Empresarial....................................................3](#_page2_x72.00_y244.17)

  
  [Servidores y Herramientas Utilizadas................................................................................3](#_page2_x72.00_y443.34)

  
  [Estructura de Red Propuesta.............................................................................................4](#_page3_x72.00_y72.00)

  
[**INSTALACIÓN SERVIDOR FIREWALL/PROXY....................................................................5**](#_page4_x72.00_y72.00)

1. [Instalación del Sistema Operativo..................................................................................5](#_page4_x72.00_y118.99)
1. [Configuración de Interfaces de Red...............................................................................5](#_page4_x72.00_y203.82)
1. [Preparación del Firewall.................................................................................................5](#_page4_x72.00_y288.65)
1. [Creación de Reglas........................................................................................................6](#_page5_x72.00_y72.00)
1. [Instalación de SQUID Proxy...........................................................................................7](#_page6_x72.00_y276.18)

[**Configuración punto de acceso........................................................................................... 8**](#_page7_x72.00_y320.35)

<a name="_page2_x72.00_y72.00"></a>Introducción

En el marco de la práctica final de este curso, nos enfrentamos al desafío de aplicar de manera integral los conocimientos adquiridos, desarrollando un plan de trabajo eficiente en equipo. Este proyecto tiene como objetivo principal la implementación de una arquitectura de red empresarial, donde la gestión de objetivos, resolución de problemas y documentación técnica juegan un papel crucial.

<a name="_page2_x72.00_y191.73"></a>Retos y Objetivos

<a name="_page2_x72.00_y244.17"></a>Implementación de la Arquitectura de Red Empresarial

- Diseño de Red: Configuración de 2 redes internas y una zona DMZ para albergar servidores públicos.
- Firewall: Desarrollo de reglas iptables para controlar el tráfico y garantizar la seguridad de la red.
- Proxy Transparente: Configuración de un proxy que permita la navegación a Internet tras validar usuarios en el servidor LDAP.
- Control de Acceso: Restricción de acceso a páginas no deseadas.
- Seguridad Inalámbrica: Implementación de una subred para clientes WIFI autorizados mediante RADIUS/LDAP.

<a name="_page2_x72.00_y443.34"></a>Servidores y Herramientas Utilizadas

- Servidor LDAP: Utilización de un servidor LDAP para la gestión de usuarios en el proxy y autenticación de clientes inalámbricos.
- LAM (ldap-account-manager): Implementación de un GUI para facilitar la configuración, gestión y mantenimiento del directorio LDAP.
- Servidor Web: Despliegue de un servidor web en la DMZ para alojar la documentación del proyecto en formato markdown.
- Servidor Radius: Autenticación de peticiones de clientes inalámbricos provenientes del punto de acceso.

<a name="_page3_x72.00_y72.00"></a>Estructura de Red Propuesta

[![Aspose-Words-9f5f01c8-5adf-4172-9b6c-3424ac8ff944-002.png](https://i.postimg.cc/zX9xfG5W/Aspose-Words-9f5f01c8-5adf-4172-9b6c-3424ac8ff944-002.png)](https://postimg.cc/kBvQ192X)

En resumen, afrontamos el desafío de crear una infraestructura que garantice la seguridad, la eficiencia operativa y el cumplimiento de requisitos específicos. El trabajo en equipo, la planificación detallada y la aplicación práctica de los conceptos aprendidos serán fundamentales para alcanzar el éxito en este proyecto.

<a name="_page4_x72.00_y72.00"></a>**INSTALACIÓN SERVIDOR FIREWALL/PROXY**

1. **Instalación<a name="_page4_x72.00_y118.99"></a> del Sistema Operativo**
   

Instala una distribución de Linux en el servidor. Puedes optar por una distribución como Ubuntu Server o CentOS, que son opciones comunes y ampliamente soportadas. En este caso usaremos ubuntu server.

2. **Configuración<a name="_page4_x72.00_y203.82"></a> de Interfaces de Red**
   

Configura las interfaces de red para reflejar la topología de tu red empresarial. Abre el archivo de configuración de red, generalmente ubicado en /etc/netplan o /etc/network/interfaces, en este caso /etc/netplan.
[![image.png](https://i.postimg.cc/VvFWSrTK/image.png)](https://postimg.cc/MfvQNTzB)


3. **Preparación<a name="_page4_x72.00_y288.65"></a> del Firewall**
   

Con el servidor configurado con las direcciones IP correspondientes, procedemos a la instalación de netfilter-persistent, la herramienta que utilizaremos para establecer las reglas de nuestro firewall. Para ello, actualizamos los repositorios y realizamos la instalación con los siguientes comandos:

```bash

sudo apt update

sudo apt upgrade

sudo apt install netfilter-persistent
```


Posteriormente, creamos el directorio /etc/iptables, que servirá para almacenar las reglas del firewall. Además, activamos el reenvío de paquetes con los siguientes comandos:

```bash

mkdir /etc/iptables

sudo netfilter-persistent save

sudo sysctl -w net.ipv4.ip\_forward=1 sudo sysctl -p
```

Con estas configuraciones, estamos listos para introducir las reglas específicas del firewall que controlarán el tráfico en nuestra red. Este paso permitirá garantizar la seguridad y eficiencia de la arquitectura de red empresarial que estamos implementando.

4. **Creación<a name="_page5_x72.00_y72.00"></a> de Reglas**

Ahora procedemos a establecer las reglas del firewall, siguiendo las directrices específicas del proyecto. Aquí se detallan las reglas según lo solicitado:

**NAT (Network Address Translation):**
```bash

sudo iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE
```

Esta regla permite realizar la traducción de direcciones de red (NAT) para el tráfico saliente. **Control de Acceso a la Red Interna:**

```bash

sudo iptables -P FORWARD DROP
```

Se establece una política predeterminada de denegar todo el tráfico de forwarding, permitiendo un control más preciso de las conexiones.

**Acceso a Internet desde Redes Internas:**
```
sudo iptables -A FORWARD -i enp0s8 -o enp0s3 -j ACCEPT

sudo iptables -A FORWARD -i enp0s9 -o enp0s3 -j ACCEPT

sudo iptables -A FORWARD -m state --state RELATED,ESTABLISHED -j ACCEPT
```
Estas reglas permiten el acceso a Internet desde las redes internas y solo aceptan paquetes establecidos o relacionados.

**Restricción de Acceso desde DMZ a Red Interna:**
```bash

sudo iptables -A FORWARD -i enp0s9 -o enp0s8 -j ACCEPT
```
Se permite el tráfico desde la DMZ al servidor web en la red interna, evitando que actúe como puerta de entrada.

**Acceso Externo solo al Servidor Web:**
```bash

sudo iptables -A FORWARD -i enp0s3 -o enp0s8 -p tcp -m multiport --dports 80,443 -j ACCEPT

sudo iptables -t nat -A PREROUTING -i enp0s3 -p tcp -m multiport --dports 80,443 -j DNAT --to-destination 172.16.1.10
```
Estas reglas permiten el acceso desde el exterior solo al servidor web y realizan la traducción de dirección de red (DNAT) para redirigir el tráfico al servidor web interno.

**Tráfico Saliente a través del Proxy:**
```bash

sudo iptables -A FORWARD -s 172.16.2.1/24 -o enp0s3 -j ACCEPT
```
Esta regla dirige el tráfico saliente desde la red interna a través del proxy. Tan solo guardamos las reglas en el archivo /etc/iptables/rules.v4 y comprobamos que las reglas funcionen como esperamos.

**sudo iptables-save > /etc/iptables/rules.v4**

Con estas reglas, se ha configurado el firewall según las especificaciones del proyecto. Asegúrate de realizar pruebas exhaustivas para verificar que el firewall funcione correctamente y cumpla con los requisitos establecidos.

5. **Instalación<a name="_page6_x72.00_y276.18"></a> de SQUID Proxy**

La instalación y configuración de nuestro proxy SQUID es un proceso breve y sencillo. A continuación, se detallan los pasos para llevar a cabo esta tarea:

**Instalamos el paquete de SQUID mediante el siguiente comando:**
```bash

sudo apt install squid
```
**Averiguamos el puerto** que está utilizando SQUID utilizando el siguiente comando y veremos que utiliza el 3128:
```bash

sudo netstat -apn | grep squid
```

**Configuración del Archivo squid.conf:**

Editamos el archivo de configuración de SQUID para ajustarlo a nuestras necesidades:
```bash

sudo nano /etc/squid/squid.conf
```
A continuación, añadimos o modificamos las siguientes líneas para habilitar la autenticación LDAP y restringir el acceso a ciertos grupos de páginas:

**auth\_param basic program /usr/lib/squid/basic\_ldap\_auth -b “ou=unidad,dc=grupo1,dc=com" -f "uid=%s" -h 172.16.2.15 auth\_param basic children 5 startup=5 idle=1**

**auth\_param basic credentialsttl 2 hours**

**acl ldap\_users proxy\_auth REQUIRED**

**acl social dstdomain .instagram.com .x.com .facebook.com acl juegos dstdomain .minijuegos.com .juegos.com**

**http\_access deny social http\_access deny juegos http\_access allow all**

Estas líneas permitirán la autenticación en la red a través de las credenciales almacenadas en la unidad organizativa de LDAP y bloquearán el acceso a ciertos grupos de páginas.

**Guardar y Salir:**

Guardamos los cambios realizados en el archivo y salimos del editor de texto. Reiniciamos el servicio y procedemos a la prueba de todo lo realizado.

Con estos ajustes en el archivo de configuración, nuestro servidor enrutador/firewall/proxy está preparado para cumplir con los requisitos de la práctica. Solo restaría configurar los clientes para que utilicen nuestro servidor como proxy, canalizando todas sus solicitudes a través del mismo.

<a name="_page7_x72.00_y320.35"></a>**Configuración punto de acceso**

**Configuración del Punto de Acceso con Autenticación Radius**

Para configurar el punto de acceso con autenticación RADIUS, sigue estos pasos detallados:

- **Restablecimiento del Punto de Acceso:**
- Presiona el botón de RESET en la parte trasera del punto de acceso durante 10 segundos para restablecerlo a la configuración de fábrica.
- **Configuración del Cliente:**
- Asigna al cliente la configuración de red y conectalo al punto de acceso directamente: IP 192.168.1.150/24, Puerta de enlace 192.168.1.245.
- **Acceso a la Interfaz de Configuración:**
- Abre el navegador y accede a la dirección 192.168.1.245. Utiliza las credenciales predeterminadas (usuario vacío, contraseña: admin) para acceder a la interfaz de configuración del punto de acceso.
- **Edición de Configuración:**
- Una vez en la interfaz de configuración, cambia el nombre del punto de acceso por "grupo1". Este será el nombre visible en tus dispositivos.
- **Configuración de Dirección IP:**
- Configura la dirección IP estática del punto de acceso: 172.16.2.20, máscara de subred 255.255.255.0, puerta de enlace 172.16.2.1.
- **Configuración de Seguridad:**
- Selecciona el modo de seguridad "Radius" para habilitar la autenticación a través de RADIUS.
- **Añadir Configuración RADIUS:**
- Añade la dirección IP de RADIUS: 172.16.2.15, puerto de RADIUS: 1812.
- **Guardar Configuración:**
- Guarda la configuración y reinicia el punto de acceso para aplicar los cambios.
- **Añadir Punto de Acceso a la Red:**
- Después de guardar la configuración, procede a añadir el punto de acceso a la red correspondiente. Esto puede implicar la conexión física del punto de acceso a la red y asegurarte de que esté alimentado correctamente.
- **Verificación desde la Red:**
- Una vez añadido a la red, verifica la conectividad desde otros dispositivos. Asegúrate de que los dispositivos puedan descubrir y conectarse al grupo1 con la nueva configuración.

Con estos pasos, hemos configurado con éxito el punto de acceso para autenticación a través de RADIUS utilizando el servidor LDAP. Asegúrate de realizar pruebas adicionales para verificar que la autenticación funcione correctamente y que los dispositivos se conecten al grupo1 sin problemas.

INSTALACIÓN Y CONFIGURACIÓN DEL <a name="_page12_x72.00_y98.45"></a>SERVIDOR WEB

1. **Instalación<a name="_page12_x72.00_y164.05"></a> de Apache**

Para comenzar, instalaremos Apache en el servidor web con el siguiente comando:
```bash

sudo apt install apache2
```
Una vez instalado, podemos verificar su funcionamiento abriendo el navegador y visitando nuestra dirección IP para visualizar el archivo predeterminado index.html de Apache.

2. **Generación<a name="_page12_x72.00_y331.14"></a> de la clave SSL**

Ahora, crearemos una clave SSL para habilitar el protocolo HTTPS. Ejecutaremos el siguiente comando para generar la clave y el certificado autofirmado:
```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt
```
Durante este proceso, se nos solicitará información que completaremos. Habilitaremos el módulo SSL.
```bash

sudo a2enmod ssl
```
3. **Configuración<a name="_page12_x72.00_y544.49"></a> del Virtual Host**

Vamos a crear un dominio en el directorio /var/www/ y no el predeterminado que es /var/www/html .

Creamos el directorio para el dominio:
```bash

sudo mkdir /var/www/grupo1
```

Ahora asignamos la propiedad del directorio con $USER.

```bash
sudo chown -R $USER:$USER /var/www/grupo1
```

Dentro de este directorio, crearemos un archivo HTML personalizado:

```bash

sudo nano /var/www/grupo1/index.html
```
Y añadiremos contenido, por ejemplo:


<h1>Grupo1</h1>


A continuación, configuraremos un archivo de host virtual para Apache:

```bash
sudo nano /etc/apache2/sites-available/grupo1.conf
```
Con el siguiente contenido:

<VirtualHost \*:443>

ServerName 172.16.2.10

DocumentRoot /var/www/grupo1

SSLEngine on

SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key

</VirtualHost>

<VirtualHost \*:80>

ServerName 172.16.2.10

Redirect / https://172.16.2.10/ </VirtualHost>

Podemos verificar la configuración con:
```bash

sudo apache2ctl configtest
```
Luego, habilitaremos el nuevo host virtual y deshabilitaremos el predeterminado:
```bash

sudo a2dissite 000-default.conf sudo a2ensite grupo1.conf
```
Finalmente, reiniciamos Apache para aplicar los cambios:
```bash
sudo systemctl restart apache2
```
Ahora, el servidor web está configurado para utilizar SSL y mostrar el contenido personalizado en el directorio del grupo1.
