Tutorial de uso de ``ffmpeg``
==============================

En el día a día es frecuente trabajar con archivos de audio y vídeo de distintos formatos. Aunque herramientas online como [YouTube](https://youtube.com) o [Vimeo](https://vimeo.com) simplifican el trabajo y la compartición de estos archivos a menudo es útil hacer un procesamiento simple de estos archivos en nuestros propios equipos. Esto tiene alguna ventajas:

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

Así, si examináramos un archivo ``.mkv`` o ``.avi`` veríamos que en realidad lo que llamamos un archivo con "una película" en realidad integra muchas cosas distintas. Por eso lo correcto es llamar a estos archivos "contenedores". En los párrafos siguientes veremos como manipular estos ficheros usando ``ffmpeg``


Instalación
=================

Para todo el tutorial vamos a necesitar dos cosas: ``ffmpeg`` y algún archivo de vídeo.

1. El ejecutable de ``ffmpeg`` puede encontrarse en su página web [http://www.ffmpeg.org](http://www.ffmpeg.org/). Si tenemos GNU/Linux simplemente necesitaremos ejecutar ``sudo apt-get install ffmpeg``.
2. Algún archivo de vídeo. Si no tenemos ninguno a mano hay películas de libre distribución como Sintel, que podremos descargar en [https://durian.blender.org/download/](https://durian.blender.org/download/). Esta película en concreto puede descargarse a resolución FullHD. Debido a que Sintel es más cortometraje que película el archivo entero en alta resolución ocupa poco más de 1 GB.
3. Algún archivo de audio. En el mismo sitio web de Sintel podemos descargar la banda sonora de la película con el nombre ``sintel-m+e.flac``.
4. Un archivo de subtítulos. Sintel también incluye diversos archivos de subtítulos de los cuales descargaremos por ejemplo la versión en español. Dicho archivo se llama ``sintel_es.srt``. En realidad la película descargada *ya incluye los subtítulos* pero nos vendran bien el tenerlos por separado para practicar algunas operaciones.

Una vez descargados ambos archivos, el ejecutable y el vídeo, pondremos ambos ficheros en el mismo directorio, abriremos el símbolo del sistema (o una consola en el caso de GNU/Linux) y teclearemos::

    ffmpeg

Deberíamos ver algo como esto:

![Ejecución de ffmpeg](img/01-ejecucion-ffmpeg.png)


Si no vemos algo similar a lo anterior deberemos investigar cual es el problema, que probablemente sea alguno de estos:

* No hemos descomprimido el fichero descargado.
* No estamos en el directorio correcto.
* No hemos escrito bien el comando.

Uso básico de ``ffmpeg``
===========================

Aunque ``ffmpeg`` es muy potente, la mayor parte del tiempo solo necesitaremos unas cuantas operaciones básicas, que indicamos a continuación


Obtención de información
---------------------------

El archivo de vídeo que hemos descargado se llama ``Sintel.2010.1080p.mkv``. Para obtener información sobre él basta con ejecutar::

    ffmpeg Sintel.2010.1080p.mkv


Deberíamos ver algo como esto::

    Input #0, matroska,webm, from 'Sintel.2010.1080p.mkv':
    Metadata:
        encoder         : libebml v1.0.0 + libmatroska v1.0.0
        creation_time   : 2011-04-25T12:57:46.000000Z
    Duration: 00:14:48.03, start: 0.000000, bitrate: 10562 kb/s
    Chapters:
        Chapter #0:0: start 0.000000, end 103.125000
        Metadata:
            title           : Chapter 01
        Chapter #0:1: start 103.125000, end 148.667000
        Metadata:
            title           : Chapter 02
        Chapter #0:2: start 148.667000, end 349.792000
        Metadata:
            title           : Chapter 03
        Chapter #0:3: start 349.792000, end 437.208000
        Metadata:
            title           : Chapter 04
        Chapter #0:4: start 437.208000, end 472.075000
        Metadata:
            title           : Chapter 05
        Chapter #0:5: start 472.075000, end 678.833000
        Metadata:
            title           : Chapter 06
        Chapter #0:6: start 678.833000, end 744.083000
        Metadata:
            title           : Chapter 07
        Chapter #0:7: start 744.083000, end 888.032000
        Metadata:
            title           : Chapter 08
    Stream #0:0(eng): Video: h264 (High), yuv420p(tv, bt709/unknown/unknown, progressive), 1920x818, SAR 1:1 DAR 960:409, 24 fps, 24 tbr, 1k tbn
    Stream #0:1(eng): Audio: ac3, 48000 Hz, 5.1(side), fltp, 640 kb/s
        Metadata:
        title           : AC3 5.1 @ 640 Kbps
    Stream #0:2(ger): Subtitle: subrip
    Stream #0:3(eng): Subtitle: subrip
    Stream #0:4(spa): Subtitle: subrip
    Stream #0:5(fre): Subtitle: subrip
    Stream #0:6(ita): Subtitle: subrip
    Stream #0:7(dut): Subtitle: subrip
    Stream #0:8(pol): Subtitle: subrip
    Stream #0:9(por): Subtitle: subrip
    Stream #0:10(rus): Subtitle: subrip
    Stream #0:11(vie): Subtitle: subrip

