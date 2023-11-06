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
2.Generar una clave privada (key):
Utiliza OpenSSL para generar una clave privada.
```bash
$sudoopenssl genpkey -algorithm RSA -out /home/alumno/key .pem -aes256
```
