---
layout: post
title: Publicar un blog con Jekyll
---

[Jekyll](https://jekyllrb.com) y [Github Pages](https://pages.github.com) me permiten publicar un blog con páginas estáticas. Primero estuve buscando plantillas de Jekyll y encontré <http://lanyon.getpoole.com> que me gustó bastante por su diseño muy simple.

Para generar el sitio realicé un fork de <https://github.com/poole/lanyon>. Tenemos que cambiar el nombre del repositorio para que coincida con el nombre del sitio. En mi caso el nombre del repositorio es **wilgustavo.github.io**.

Aparentemente esta plantilla no está actualizada con la nueva versión de Jekyll, ya que al iniciar el sitio GitHub me envía una alerta: 

    Your site is using the `relative_permalinks` configuration option...

Para arreglar este problema tuve que editar el archivo *_config.yml* quitando la opción de *relative_permalinks*.

También tube que modificar los enlaces de la página *index.html* 

Todos los cambios necesario los encontré en el siguiente pull request: <https://github.com/poole/lanyon/pull/149/files>   

