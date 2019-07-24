---
layout: post
title: Apuntes de cURL
---

*cURL* es una potente herramienta de línea de comandos para realizar peticiones a servicios de red y soporta la mayoría de protocolos.

Se puede utilizar en muchas situaciones tanto com para probar servicios, descargar archivos, depurar problemas de red o incluirlos en scripts.

## Uso básico

La forma básica de realizar una petición es:
```
curl <url>
```

Por ejemplo:
```
curl http://httpbin.org/json
```

Esta acción genera una petición GET a la URL http://httpbin.org/json y muestra la respuesta por pantalla.

Para ver la opciones disponibles de este comando ingresar
```
curl -h
```

## Descargar archivos
En lugar de mostrar la respuesta por pantalla podemos indicar que los datos se guarden en un archivo. Indicamos la opción -o con el nombre del archivo. Si el archivo existe se sobrescribe.
```
curl http://httpbin.org/json  -o datos.json
```

## Encabezados
Para agregar encabezados en la petición utilizar la opción -H. Si necesitamos configurar varios encabezados tenemos que repetir la opción -H para cada línea del encabezado.
```
curl http://httpbin.org/image -H 'accept: image/jpeg' -o imagen.jpg
```

Para ver los encabezados que retorna el servidor ingresar la opción -i
```
curl -i http://httpbin.org/json
```

## Ver los datos de transacción
Con la opción -v podemos ver el detalle de la transacción. En la pantalla se despliega todas las tramas. Las sentencias que comienzan con > representan los datos que se envían.
Las líneas que comienzan con < son las respuestas del servidor. Las líneas con * son anotaciones internas del comando.

```
curl -v http://httpbin.org/json
```

Un truco para ver las tramas pero no mostrar los datos es enviar la respuesta a /dev/null:
```
curl -vo NUL http://httpbin.org/json #WINDOWS
```

```
curl -vo /dev/null http://httpbin.org/json #Linux/MacOS
```

Para ocultar la barra de progreso incluimos la opción -s
```
curl -vo /dev/null http://httpbin.org/json
```

El comando curl por defecto no retorna con error cuando el servidor responde con códigos de estado de error. Si tenemos un script y necesitamos que de error al producirse un problema,
tenemos que utilizar el parámetro -f
```
curl https://httpbin.org/status/401 -f
```


## Enviar datos
Enviar datos al servidor con la opción -d.
```
curl https://httpbin.org/post \
     -d '{"user":"test@example.com", "tags": ["example", "test"]}' \
     -H 'Content-Type: application/json'
```

Cuando se envía datos se establece por defecto el método post es lo mismo que indicar lo siguiente:

```
curl https://httpbin.org/post \
    -d '{"user":"test@example.com", "tags": ["example", "test"]}' \
    -H 'Content-Type: application/json' -X POST
```

La opción -X es para indicar el método HTTP.

Los datos los podemos almacenar en una archivo si son muy grandes para enviarlos por línea de comandos. Indicamos con @ el nombre de archivo.
```
curl https://httpbin.org/post -d @datos.json \
      -H 'Content-Type: application/json'
```

Enviamos un archivo con el parámetro -F y la ruta al archivo.
```
curl -F file=@imagen.jpg http://httpbin.org/post
```

Para enviar datos de un formulario es posible utilizar la opción data-urlencode que codifica los datos con el formato url-encode.
```
curl https://httpbin.org/post \
       --data-urlencode "email=test@example.com" \
       --data-urlencode "name=Jon Snow"
```

## Autenticación
Para utilizar una autenticación de usuario básica, utilizar el parámetro -u con usuario y contraseña separado por dos puntos. Esta opción envía al servidor un encabezado de autenticación
con las credenciales codificadas en Base64
```
curl http://httpbin.org/get -u prueba:abc123
```

El mismo comportamiento obtenemos enviando el usuario y contraseña directamente en la URL con el formato usuario:password@url
```
curl http://prueba:abc123@httpbin.org/get
```

## Métodos HTTP
Podemos cambiar el método por defecto utilizando el parámetro -X

Para realizar una petición PUT y modificar el estado de un recurso utilizar el parámetro -X PUT

```
curl https://httpbin.org/put \
     -X PUT \
     -d '{"user":"test@example.com", "tags": ["example", "test"]}' \
     -H 'Content-Type: application/json'
```

Para borrar un recurso utilizar el parámetro -X DELETE

```
curl https://httpbin.org/delete -X DELETE
```

Para ver las opciones del recurso.

```
curl -i -X OPTIONS http://httpbin.org/get
```

## Tiempo de espera
Para limitar el tiempo de espera podemos utilizar el parámetro -m con la cantidad de segundos

```
curl https://httpbin.org/delay/10 -m 5
```

El comando responde con error cuando supera el tiempo de espera.

## Mas información

* [ec.haxx.se](https://ec.haxx.se)
* [curl.haxx.se/docs/manpage.html](https://curl.haxx.se/docs/manpage.html)
