---
layout: post
title: Arquetipo básico para Maven
---

Me costaba bastante encontrar una buena plantilla para proyectos Maven, hasta que dí con un muy buen [quickstart para Java 8](https://github.com/mikolak-net/java8-quickstart-archetype)

Para crear un proyecto con Maven escribir en la línea de comandos:

```
mvn archetype:generate -B
 -DarchetypeGroupId=pl.org.miki
 -DarchetypeArtifactId=java8-quickstart-archetype
 -DarchetypeVersion=1.0.0
 -DgroupId=com.example -DartifactId=project
 -Dversion=1.0
 -Dpackage=com.example.project
```

Incluye una opción *compilerMode* para indicar la versión de Java (por defecto utiliza JDK8). También tiene una opción *testLibrary* para cambiar la librería de testing, que por defecto utiliza una versión de JUnit 4.

Otras mejoras con respecto a otros arquetipos es que crea directorios *main* y *test* pero no crear fuentes de ejemplos, que siempre terminamos borrando.

* Mas infomación en la página de GitHub: [https://github.com/mikolak-net/java8-quickstart-archetype](https://github.com/mikolak-net/java8-quickstart-archetype)

