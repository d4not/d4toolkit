# Caso de uso: Port Forwarding

## 1. Identificar puertos locales activos

Para verificar si hay algún puerto activo en localhost (127.0.0.1) que no sea accesible públicamente, podemos utilizar:

```
ss -tulpn
```

O también:

```
(netstat -punta || ss --ntpu) | grep "127.0"
```

## 2. Inspeccionar qué servicio usa un puerto específico

Si detectamos un puerto interesante, podemos ver qué proceso o servicio lo está utilizando:

```
lsof -i :PUERTO
```

Reemplaza PUERTO por el número real del puerto que encontraste (ej. 3306, 5000, etc.).

## 3. Establecer conexión SSH y configurar port forwarding

Supongamos que ya hemos comprometido una máquina víctima y podemos subir archivos o ejecutar comandos. Vamos a generar un par de claves SSH desde la máquina atacante.

### Máquina atacante

```
ssh-keygen -f nombre_usuario_key -t rsa
```

Esto creará dos archivos:

- nombre_usuario_key (clave privada)
- nombre_usuario_key.pub (clave pública)

Un ejemplo del contenido de nombre_usuario_key.pub podría ser:

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDil6f/wy3UZ6RSUORVz8MoJMf3nJnwLVK7L7GnbMdrnyxB++NSxNOjOYoiUvRty7oxIk4fiMDW+m0+jIEk/+gFVsBCDYVOcbESh+fSavE9VYAln1j3JSQc+YZZ9TFQoEPtuVj/+PaLvp5MA+HROmMncDO4//kljgHgTAqkGaxSKCk4rI6gemmh6IdqFlyLCmGMIsex93TggZ4cAk/X046q3+A3nLjMNEsgN4n2fx3/IF8W8HO8jbNxLnSgeXooG7plvYFopgcFRCFi7/7/zxgD/rhDOsxbmo0ON6mSPOPjdlxB4KyIdQ0RWjy3Y5AZ1/YrY25kVxa7Je8KS6/5a9AI+Gjy9xayR6UOdcy4s+QvvPKezM0SC1EUk5JM4r/a6OeiduJqX+A4/l0jUiWLJ3h82zo3bKdifiDrYjPNO2oaUfycN9qEapzHa59fRcZgDD3hqSKWcut6b6anEf0Ryjw9CrdxFijIgTNE8pe9PVjuvNSxeAhDtt+xustl1CNOYFs= daniel@parrot
```

### En la máquina víctima

Sube la clave pública (nombre_usuario_key.pub) y guárdala en el archivo:

```
~/.ssh/authorized_keys
```

Asegúrate de que el archivo tenga los permisos correctos:

```
chmod 600 ~/.ssh/authorized_keys
```

Si se hizo correctamente, ahora podrás conectarte a la máquina víctima sin contraseña:

```
ssh nombre_usuario@ip_maquina_victima -i nombre_usuario_key
```

## 4. Realizar port forwarding

Ahora puedes crear un túnel entre tu máquina y el servicio que está escuchando en la víctima localmente:

```
ssh -i nombre_usuario_key -L puerto_local:127.0.0.1:puerto_remoto nombre_usuario@ip_maquina_victima
```

Por ejemplo, si el servicio está corriendo en el puerto 5000 de la víctima, puedes exponerlo a tu localhost:

```
ssh -i daniel_key -L 5000:127.0.0.1:5000 daniel@192.168.1.100
```

Luego podrás acceder al servicio directamente desde tu navegador o herramientas locales en http://127.0.0.1:5000

## Notas finales

- Este método es útil para interactuar con servicios que solo están escuchando en 127.0.0.1 de la víctima.
- También puedes usar este túnel como base para proxychains, escaneo interno o explotación posterior.
