1.generar certificado
autofirma
instalación(Apache)
comprobación


## Introducción
Los certificados digitales sirven para acreditar quien eres en el mundo de intrenet es una manera adicional y segura de acreditarte con los siguientes pasos vamos a generar un certficado en ubuntu:

1.instalamos openssl
```bash
$sudo apt install openssl
```
2.Generar una par de claves privada y publica (key):
Utiliza OpenSSL para generar una clave privada.
```bash
$sudo sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt
```
3.Configurar apache para que utilice SSL
Ahora modificaremos /etc/apache2/sites-available/default-ssl.conf que es el archivo de configuración de ssl por defecto de apache donde meteremos la clave privada y publica que generamos con el comando anterior.
Dentro del archivo metemos el siguientes líneas:
```bash
SSLEngine on

     SSLCertificateFile      /etc/ssl/certs/apache-selfsigned.crt
    SSLCertificateKeyFile   /etc/ssl/private/apache-selfsigned.key
```
4.Ajustamos el firewall para que admita trafico https
```bash
    sudo ufw allow 'Apache Full'
    sudo ufw delete allow 'Apache'
```
5.Por ultimo debemos habilitar el archivo de configuración de ssl con el comando a2ensite:
```bash
cd /etc/apache/sites-avaible/
$sudo a2ensite default-ssl
```

## Resultado
1.Ahora nos metemos dentro de la configuración de nuestro navegador e importamos nuestro certificado.
![firefox](https://github.com/Gheorghe01/gheorghe01.github.io/assets/145337384/5b64d7b7-60cd-4b08-b49a-d0ef69a896ab)

![firefox2](https://github.com/Gheorghe01/gheorghe01.github.io/assets/145337384/48b510cf-a125-47e8-a133-d882f910496d)
