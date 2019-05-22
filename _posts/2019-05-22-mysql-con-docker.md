---
layout: post
title: Crear una base de datos MySQL con Docker
---

Para instalar una base de datos MySQL podemos utilizar una imagen docker.

Primero en una carpeta creamos un archivo *docker-compose.yml*.

```yaml
version: '3.3'

services:
  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_DATABASE: 'db'
      MYSQL_USER: 'user'
      MYSQL_PASSWORD: 'password'
      MYSQL_ROOT_PASSWORD: 'abc123'
    ports:
      - '3306:3306'
    expose:
      - '3306'
    volumes:
      - ./data:/var/lib/mysql
    command:
      mysqld --innodb-flush-method=O_DSYNC --innodb-use-native-aio=0  --default-time-zone=-3:00
```

La configuración del contenedor tiene los siguientes atributos:
* **image**: Utilizamos una imagen de MySQL versión 5.7
* **environment**: Variables de entornos utilizada por el contenendor. Se establecen los usuarios y contraseña.
* **ports**: Los puertos de red que se utilizan para conetarse con el contenedor. El puerto 3306 se utiliza normalemte en una base de datos MySQL.
* **volumes**: Configurar un volumen o enlace a una carpeta.
* **command**: Se sobreescribe el comando que se ejecuta al arrancar el contendor. En este caso se pasan parámetros adicionales al motor de base de datos.

Como se indica en el atributo *volumes* se creará una carpeta *data* para almacenar las bases de datos. Esta carpeta se crea en el mismo directorio donde se encuentra el archivo de configuración.

Por último se ejecuta el comando docker-compose para iniciar el contenedor.
```
docker-compose up
```





