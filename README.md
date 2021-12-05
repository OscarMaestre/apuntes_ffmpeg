Tutorial de uso de ``ffmpeg``
==============================

En el día a día es frecuente trabajar con archivos de audio y vídeo de distintos formatos. Aunque herramientas online como `YouTube <https://youtube.com>` o `Vimeo <https://vimeo.com>` simplifican el trabajo y la compartición de estos archivos a menudo es útil hacer un procesamiento simple de estos archivos en nuestros propios equipos. Esto tiene alguna ventajas:

* En líneas generales es más rápido hacer el procesamiento en local.
* Disponemos de muchas más opciones.
* Tenemos el máximo control sobre la privacidad del archivo.

Conceptos básicos
----------------------

Hoy en día, cuando descargamos un vídeo lo normal es que dentro de él pueda haber varios **flujos**. Estos flujos pueden ser de muchos tipos.

* Vídeo.
* Audio
* Subtítulos
* Información sobre capítulos.
* Etc...


Así, al visualizar un archivo nuestro software de reproducción analizará el archivo e irá extrayendo y sincronizando los sucesivos flujos para que podamos ver las imágenes, a la vez que escuchamos el sonido, nos muestra el subtítulo correcto, etc...

Así, si examináramos un archivo ``.mkv`` o ``.avi`` veríamos que en realidad lo que llamamos un archivo con "una película" en realidad integra muchas cosas distintas. Por eso lo correcto es llamar a estos archivos "contenedores". 


Instalación
=================

