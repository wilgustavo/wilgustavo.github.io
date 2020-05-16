---
layout: post
title: Crear imagenes Docker con Jib
---

Cuando creamos imágenes de Docker con nuestros programas queremos aplicar las mejores prácticas.

- Ocupe el mínimo espacio
- Construcción en etapas.
- Uso de cache
- Dependencias actualizadas.
- Sin usuario root

Pero crear una imagen que aplique estas pautas es bastante complicado. Por suerte tenemos JIB, una herramienta open-source que automatiza la construcción de imágenes sin tener que escribir un Dockerfile. Jib se puede usar como un plugin de Maven o Gradle.

Para utilizar JIB en Maven, editar el archivo pom.xml y agregar el siguiente plugin

```xml
<plugins>
    <plugin>
        <groupId>com.google.cloud.tools</groupId>
        <artifactId>jib-maven-plugin</artifactId>
        <version>2.2.0</version>
        <configuration>
            <to>
                <image>docker.io/wilgustavo/hola</image>
                <tags>
                    <tag>0.1</tag>
                    <tag>latest</tag>
                </tags>
            </to>
        </configuration>
    </plugin>
</plugins>
```

El plugin requiere la siguiente configuración:

- **to**: El nombre de la imagen que va a generar. Tiene el prefijo docker.io porque se despliega en Docker Hub. Aquí podemos elegir cualquier registro público o privado.
  También podemos indicar una o varias etiquetas.
- **from**: En forma opcional podemos indicar la imagen base. Si no indicamos nada, se parte de la imagen gcr.io/distroless/java que solo tiene JRE y se han quitado el shell y las aplicaciones de Linux.

Para compilar el proyecto y generar la imagen en local, escrimos:

```
mvn compile jib:Dockerbuild
```

En cambio si queremos subir nuestra imagen en el registro de Docker:

```
mvn compile jib:build -Djib.to.auth.username=user -Djib.to.auth.password=pass
```

En este caso no necesitamos tener un proceso de Docker corriendo en nuestra PC. En forma directa genera la imagen y la sube al registro.

En conclusión, utilizando Jib obtenemos muchas ventajas:

- Con Maven automatizamos todo el despliegue.
- No necesitamos Docker instalado ni escribir Dockerfiles
- La construcción es muy rápida ya que separa nuestra aplicación en varias capas.

Mas información en la página del proyecto [Jib](https://github.com/GoogleContainerTools/jib).
