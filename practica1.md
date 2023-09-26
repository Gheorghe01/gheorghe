# Ejercicio 1: comunicación entre servidor cliente

Empezamos con abrir los puertos 514 TCP/UDP respectivamente con el comando ufw allow 514/tcp and udp.

```bash
$ufw allow 514/tcp
$ufw allow 514/udp
```

Después entramos en el archivo de configuración /etc/rsyslog.conf para descomentar las líneas "16,17,20,21,24,25" para despues en la línea 7 poner *.*@@"ip del servidor":514 que en mi caso era mi compañero Adrian con la IP 192.168.12.13:514.

Por ultimo solo quedaria hace una comprobación enviando al servidor una mensaje cin el comando tail syslog.



