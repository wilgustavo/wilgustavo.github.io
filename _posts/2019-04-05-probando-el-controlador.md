---
layout: post
title: Probando el controlador
---

Escribimos un controlador que retorna un mensaje, ahora lo vamos a probar utilizando test unitario.

Primero debemos indicar las dependencias a las librerías de test en el archivo *pom.xml*

Agregamos estas líneas dentro del elemento *dependencies*

```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
```

Este conjunto de librerías contiene **JUnit** y herramientas de testing.

En el directorio *src/test/java* creamos el paquete *com.prueba.controller* y la clase *HolaControllerTest*.

```java
package com.prueba.controller;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;

@RunWith(SpringRunner.class)
@WebMvcTest(HolaController.class)
public class HolaControllerTest {

  @Autowired
  private MockMvc mockMvc;

  @Test
  public void saludarDeberiaRetornarUnMensaje() throws Exception{
    mockMvc
      .perform(get("/hola"))
      .andExpect(status().isOk())
      .andExpect(content().string("Mi vieja mula ya no es lo que era"));
  }
}
```

La anotación *@WebMvcTest* carga el controlador que necesitamos probar y configura a *MockMvc* para realizar peticiones web sin necesidad de iniciar un servidor.

Para iniciar la prueab ejecutamos en linea de comandos:

    mnv test

El codigo fuente lo pueden obtener en el [repositorio git](https://github.com/wilgustavo/hola-spring/tree/test-controller).
