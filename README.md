# Pontify_Assets_Docs
* Este repositorio contiene el **checklist de seguridad**, la **matriz de roles y permisos**, y el **plan de pruebas por criterio de aceptación**. También contiene las referencias que se usaron para montar a mano la base del campus 3d de la UPB...

-------------------------------------------------//--------------------------------------------------
* Consideraciones acerca de la implementación del Campus 3D.

**NOTA:** Las referencias de la carpeta "ReferenciasCampus" están en formato .pur, que es propio de un software de manejo referencial, ofrece un canvas infinito en el que se pueden pegar imágenes fácilmente y configurar la ventana de la aplicación para que aparezca en frente de otras aplicaciones, el software es gratis, con una donación completamente opcional.

El modelado del campus se realizó en un software 3D Open Source gratuito llamado "Blender", medido a una escala aproximada de 1:10 usando herramientas como Google Maps y Google Earth en vista satelital, permitiendo obtener las dimensiones horizontales principales de cada bloque de la universidad. En cuanto a las dimensiones verticales, estas fueron estimadas asumiendo la altura de cada piso (3 metros o 3.5 metros) y el grosor de cada suelo como tal (0.25 o 0.3 metros).

Adicionalmente se usó la versión del Street View 2012 del campus para trazar los detalles más complejos como en los bloques 6, 12 y 3, y también para poder ver con mayor claridad bloques altamente tapados por árboles como los bloques 8 y 9.

Cabe resaltar que por cuestiones de tiempo, nuestro Product Owner conectó Claude Code a un servidor MCP de Blender para añadir los detalles de manera más eficiente, sacrificando precisión (hay varias alucinaciones). Sin embargo el sistema "/meetingPoint" está lo suficientemente modularizado para facilitar en cualquier momento el cambio del campus 3d, de manera en que se conserve la funcionalidad de selección sin requerir pruebas adicionales.