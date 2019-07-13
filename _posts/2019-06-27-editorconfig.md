---
layout: post
title: Editorconfig
---

Me gusta mucho trabajar en Java con [VSCode](https://code.visualstudio.com). Sin embargo tiene, por defecto, un estilo muy orientado al desarrollo Web y con JavaScript. Un problema es que configura una indentación de 2 espacios, algo que no es común en Java.

En lugar de configurar directamente el IDE estoy utilizando un archivo de configuración que me permite exportar mis preferencias de estilo a cualquier editor que soporte este estándar. Este estándar es [EditorConfig](https://editorconfig.org).

Para que el IDE tome las configuraciones que deseamos solo tenemos que crear un archivo *.editorconfig* en el directorio raíz del proyecto.

Mi configuración para proyectos de Java es la siguiente:

```properties
# top-most EditorConfig file
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
indent_style = space

[*.{java,xml}]
indent_size = 4
trim_trailing_whitespace = true

```

Me permite definir como debe comportase el IDE cuando tiene que formatear el código. Puedo definir el tipo de indentación, el espacio, los saltos de líneas, entre otras propiedades. Y lo más importante: ya queda incluido en el código fuente, de esta manera se puede cambiar el IDE y no alterar las propiedades de estilo.

