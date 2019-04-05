---
layout: post
title: Hola Spring!
---

Vamos a iniciar un proyecto que consiste en una aplicación web mínima utilizando Spring Boot.

Requerimos tener instalado JDK 8 y Maven.

Para comenzar escribimos en la línea de comandos la siguiente sentencia:

```
mvn archetype:generate -DgroupId=com.prueba
-DartifactId=hola
-DarchetypeArtifactId=maven-archetype-quickstart
-DinteractiveMode=false
```

El parámetro *groupId* indica el paquete del proyecto y *artifactId*, el nombre. Maven creará un directorio con el nombre del proyecto y tendrá la siguiente estructura separando los fuentes del programa con los de test:

    |   pom.xml
    +---src
    |   +---main
    |   |   +---java
    |   |       +---com
    |   |           +---prueba
    |   |                   App.java
    |   |
    |   +---test
    |       +---java
    |           +---com
    |               +---prueba
    |                       AppTest.java

Genera además el archivo pom.xml que utiliza Maven para configurar el proyecto. (Necesitamos borrar el archivo AppTest.java para no tener problemas posteriormente).

Para crear un proyecto de Spring Boot necesitamos modificar el archivo *pom.xml*:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.prueba</groupId>
  <artifactId>hola</artifactId>
  <packaging>jar</packaging>
  <version>1.0-SNAPSHOT</version>

  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.1.3.RELEASE</version>
  </parent>

  <properties>
    <java.version>1.8</java.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
  </dependencies>

  <build>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>
</project>
```

Incluimos por ahora solo la dependencia de Spring Web que necesitamos para crear una aplicación web simple. Con estos datos Maven puede descargar todas las librerías que necesitamos para compilar el proyecto.

Ahora podemos modificar el fuente *App.java*.

```java
package com.prueba;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class App {

    public static void main(String[] args){
        SpringApplication.run(App.class, args);
    }
}
```

Cuando incluimos la anotación *@SpringBootApplication* estamos indicando que se active el sistema de auto configuración de Spring Boot y rastree todos los elementos de la aplicación en el árbol de clases, así no tenemos que configurar nada nosotros(o muy poco).

Al llamar al método *SpringApplication.run* iniciamos la aplicación.

Aunque es una aplicación web, no necesitamos incluir una archivo *web.xml*.

Por último nos queda incluir un "endpoint", para ello creamos un paquete *com.prueba.controller* y la clase *HolaController*. Con el nombre indicamos que es un controlador, es decir que recibe las peticiones web del usuario.

```java
package com.prueba.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HolaController {

  @GetMapping("/hola")
  public String saludar() {
    return "Mi vieja mula ya no es lo que era";
  }
}
```

Mediante la anotación *@RestController* indicamos que la clase es un controlador. Anotamos el método *saludar* con *@GetMapping* para indicar que se debe ejecutar cuando se reciba una petición HTTP con el método *GET* en la url */hola*.

Eso es todo! Ejecutamos la aplicación escribiendo la siguiente sentencia en la linea de comandos:

    mvn spring-boot:run

Probamos la aplicación abriendo la página <http://localhost:8080/hola> en el navegador.

Los fuentes de este proyecto los tienes en el [repositorio git](https://github.com/wilgustavo/hola-spring/tree/hola).
