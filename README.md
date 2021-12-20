---
title: Tutorial de uso de ffmpeg
author: Oscar G.G
date: Diciembre de 2021
---

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

Una vez descargados ambos archivos, el ejecutable y el vídeo, pondremos ambos ficheros en el mismo directorio, abriremos el símbolo del sistema (o una consola en el caso de GNU/Linux) y teclearemos:

    ffmpeg

Deberíamos ver algo como esto:

    ffmpeg version N-104766-g3a9861e22c-20211205 Copyright (c) 2000-2021 the FFmpeg developers
    built with gcc 10-win32 (GCC) 20210610
    configuration: --prefix=/ffbuild/prefix --pkg-config-flags=--static --pkg-config=pkg-config --cross-prefix=x86_64-w64-mingw32- --arch=x86_64 --target-os=mingw32 --enable-gpl --enable-version3 --disable-debug --disable-w32threads --enable-pthreads --enable-iconv --enable-libxml2 --enable-zlib --enable-libfreetype --enable-libfribidi --enable-gmp --enable-lzma --enable-fontconfig --enable-libvorbis --enable-opencl --enable-libvmaf --disable-libxcb --disable-xlib --enable-amf --enable-libaom --enable-avisynth --enable-libdav1d --enable-libdavs2 --disable-libfdk-aac --enable-ffnvcodec --enable-cuda-llvm --enable-frei0r --enable-libgme --enable-libass --enable-libbluray --enable-libmp3lame --enable-libopus --enable-librist --enable-libtheora --enable-libvpx --enable-libwebp --enable-lv2 --enable-libmfx --enable-libopencore-amrnb --enable-libopencore-amrwb --enable-libopenh264 --enable-libopenjpeg --enable-librav1e --enable-librubberband --enable-schannel --enable-sdl2 --enable-libsoxr --enable-libsrt --enable-libsvtav1 --enable-libtwolame --enable-libuavs3d --disable-libdrm --disable-vaapi --enable-libvidstab --enable-vulkan --enable-libglslang --enable-libplacebo --enable-libx264 --enable-libx265 --enable-libxavs2 --enable-libxvid --enable-libzimg --enable-libzvbi --extra-cflags=-DLIBTWOLAME_STATIC --extra-cxxflags= --extra-ldflags=-pthread --extra-ldexeflags= --extra-libs=-lgomp --extra-version=20211205
    libavutil      57. 10.101 / 57. 10.101
    libavcodec     59. 14.100 / 59. 14.100
    libavformat    59.  9.102 / 59.  9.102
    libavdevice    59.  0.101 / 59.  0.101
    libavfilter     8. 19.100 /  8. 19.100
    libswscale      6.  1.101 /  6.  1.101
    libswresample   4.  0.100 /  4.  0.100
    libpostproc    56.  0.100 / 56.  0.100
    Hyper fast Audio and Video encoder
    usage: ffmpeg [options] [[infile options] -i infile]... {[outfile options] outfile}...

    Use -h to get full help or, even better, run 'man ffmpeg'


Si no vemos algo similar a lo anterior deberemos investigar cual es el problema, que probablemente sea alguno de estos:

* No hemos descomprimido el fichero descargado.
* No estamos en el directorio correcto.
* No hemos escrito bien el comando.

Uso básico de ``ffmpeg``
===========================

Aunque ``ffmpeg`` es muy potente, la mayor parte del tiempo solo necesitaremos unas cuantas operaciones básicas, que indicamos a continuación


Obtención de información
---------------------------

El archivo de vídeo que hemos descargado se llama ``Sintel.2010.1080p.mkv``. Para obtener información sobre él basta con ejecutar:

    ffmpeg Sintel.2010.1080p.mkv


Deberíamos ver algo como esto:

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

¿Como se interpreta esto? Bien, ``ffmpeg`` ha recibido un fichero de entrada, al que ha denominado ``Input #0`` (cabe destacar que ``ffmpeg`` puede aceptar varios ficheros de entrada para por ejemplo tomar el vídeo del fichero 0 y el audio del fichero 1). Dentro de ``Input #0`` encontramos lo siguiente:

* Metadatos.
* Duración del fichero.
* Información sobre capítulos: para cada uno de ellos podemos ver su número y donde empiezan y acaban. Observar que aparecen indicados como ``Chapter #0:4`` lo que significa *capítulo 4 del fichero de entrada 0*
* Hay un flujo con el número 0, que es de vídeo, codificado con el códec h264.
* Hay un flujo con el número 1, que es de audio, codificado con el códec ac3.
* Hay varios flujos de subtítulos, con números entre el 2 y el 11, en los que se ha indicado el idioma de escritura.

Conversiones básicas
----------------------

Si solo queremos convertir un fichero que usa un formato de contenedor en otro distinto, el proceso es simple. Por ejemplo, para convertir nuestro fichero a otro que utilice AVI como contenedor solo tendremos que escribir esto:

    ffmpeg -i Sintel.2010.1080p.mkv Sintel.avi

Este comando toma un fichero de entrada (opción ``-i``) y genera un fichero de salida (con el nombre que escribamos al final). Una vez lanzado el comando, ``ffmpeg`` mostrará información sobre los ficheros de entrada y despues  empezará a realizar la conversión. En general **convertir vídeo es un proceso muy lento que se ve muy influido por la potencia del microprocesador del que se disponga**. Pasado un tiempo tendremos ambos ficheros y podremos abrirlos con cualquier reproductor multimedia. 

Despues de obtener nuestro archivo AVI podemos examinarlo con este comando:

    ffmpeg -i Sintel.avi

Y podremos ver algo como esto:

    Input #0, avi, from 'Sintel.avi':
    Metadata:
        software        : Lavf59.9.102
    Duration: 00:14:48.07, start: 0.000000, bitrate: 1732 kb/s
    Stream #0:0: Video: mpeg4 (Simple Profile) (FMP4 / 0x34504D46), yuv420p, 1920x818 [SAR 1:1 DAR 960:409], 1592 kb/s, 24 fps, 24 tbr, 24 tbn
    Stream #0:1: Audio: mp3 (U[0][0][0] / 0x0055), 48000 Hz, stereo, fltp, 128 kb/s
        Metadata:
        title           : AC3 5.1 @ 640 Kbps


Como vemos, ha habido algunos cambios:

* El flujo original de vídeo estaba codificado con el códec h264. En el fichero final está codificado con mpg4.
* El audio original estaba codificado con ac3. El final lo está en mp3.

¿A qué se debe esto? Muy sencillo, ``ffmpeg`` tiene sus propios códecs por defecto, así que si no indicamos nada, **convertirá los flujos originales a los formatos por defecto, lo que puede ser bastante lento**

Códecs
--------------

Un códec es un **CO**dificador-**DEC**odificador (en realidad en español se dice "descodificador") de audio o de vídeo. Codificar algo consiste en tomar los datos originales y transformarlos en otra secuencia de datos distinta y descodificar implica tomar la secuencia de datos transformada y recuperar los datos originales. Como en muchos casos interesa reducir el tamaño de los datos muchos códec tienen la capacidad de comprimir. Además, en algunos casos, la compresión puede ser mejor si aceptamos cierta pérdida de calidad se puede decir que hay "compresión con pérdida" y "compresión sin pérdida".

Ocurre también que hay literalmente cientos de mecanismos para hacer esta tarea y en ocasiones podremos observar que algunos ficheros han sido codificados con un códec que nuestro programa, nuestro móvil o nuestra TV no tiene lo que daría lugar a que **no se pueda ver u oír (o ambos) el fichero**. En estos casos nos interesará **"transcodificar"** uno o varios flujos del fichero.

Volvamos a ver qué códecs había usado ``ffmpeg`` en nuestro fichero:

    Input #0, avi, from 'Sintel.avi':
    Metadata:
        software        : Lavf59.9.102
    Duration: 00:14:48.07, start: 0.000000, bitrate: 1732 kb/s
    Stream #0:0: Video: mpeg4 (Simple Profile) (FMP4 / 0x34504D46), yuv420p, 1920x818 [SAR 1:1 DAR 960:409], 1592 kb/s, 24 fps, 24 tbr, 24 tbn
    Stream #0:1: Audio: mp3 (U[0][0][0] / 0x0055), 48000 Hz, stereo, fltp, 128 kb/s
        Metadata:
        title           : AC3 5.1 @ 640 Kbps


Como vemos, ``ffmpeg`` ha recodificado el vídeo y lo ha pasado de "h264" a "mpeg4" y ha recodificado el audio de "ac3" a "mp3". Si deseamos usar un codec de audio o de vídeo concreto podemos pedirle a ``ffmpeg`` que lo utilice usando las opciones ``-acodec <codec_de_audio>`` o ``-vcodec <codec_de_video>``

Podemos ver la lista de codecs de ``ffmpeg`` usando esto:

    ffmpeg -codecs

Lo que nos mostrará una lista (muy muy larga) de todos los codec que el programa maneja. Al comienzo de la lista veremos algo como esto, que nos explica lo que significa la información que vemos (incluimos una traducción del significado):

    V..... = Video (el códec se usa para codificar/descodificar vídeo)
    A..... = Audio (el códec se usa para codificar/descodificar audio)
    S..... = Subtitle (el códec se usa para codificar/descodificar subtítulos)
    .F.... = Frame-level multithreading (los fotogramas se pueden procesar en paralelo)
    ..S... = Slice-level multithreading (se pueden procesar en paralelo trozos de un fotograma)
    ...X.. = Codec is experimental (no requiere traducción)
    ....B. = Supports draw_horiz_band (el codec acepta técnicas que mejoran la eficiencia de caché)
    .....D = Supports direct rendering method 1 (el codec acepta la aceleración hardware)

Cada codec tiene sus ventajas e inconvenientes. Si no se sabe muy bien cual usar una buena elección es usar "h264" para vídeo y "mp3" para audio. Al ser tan extendidos es más probable que el fichero generado con estos códecs pueda verse y oírse bien en casi cualquier dispositivo.

Volviendo a nuestro ejemplo, podría ocurrir que el fichero original ``Sintel.2010.1080p.mkv`` utilizase codecs correctos y solamente hubiésemos querido modificar el contenedor. Recordemos que nuestro ejemplo anterior ``ffmpeg -i Sintel.2010.1080p.mkv Sintel.avi`` produjo una transcodificación tanto del audio como del vídeo que conllevó bastante tiempo. 

Por tanto, para simplemente cambiar de formato de contenedor podemos usar esto (suele ser recomendable indicar en el nombre de fichero los códecs utilizados):

    ffmpeg -i Sintel.2010.1080p.mkv -acodec copy -vcodec copy Sintel-h264-ac3.avi

Sin embargo obtendremos un error como este:

    H.264 bitstream malformed, no startcode found, use the video bitstream filter 'h264_mp4toannexb' to fix it ('-bsf:v h264_mp4toannexb' option with ffmpeg)
    av_interleaved_write_frame(): Invalid data found when processing input

Bien, aunque la idea era buena, aprendemos que **no todos los contenedores aceptan directamente todas las posibilidades**. En concreto, ocurre que AVI es un formato de contenedor que se diseñó antes que h264 y AVI no ofrece soporte directo al sistema de codificación que proporciona h264 por lo que usaremos el mismo codec de video que usó ``ffmpeg`` en la primera conversión y dejaremos el códec de audio en ac3:

    ffmpeg -i Sintel.2010.1080p.mkv -acodec copy -vcodec mpeg4 Sintel-mpeg4-ac3.avi

Ahora la conversión sí se producirá y será más rápida que la realizada inicialmente (que recordemos convirtió el vídeo de h264 a mpeg4 y el audio de ac3 a mp3). Si consultamos los códecs usados en el fichero final con esto:

    ffmpeg -i Sintel-mpeg4-ac3.avi

Veremos que el resultado es este:

    Input #0, avi, from 'Sintel-mpeg4-ac3.avi':
    Metadata:
        software        : Lavf59.9.102
    Duration: 00:14:48.03, start: 0.000000, bitrate: 2242 kb/s
    Stream #0:0: Video: mpeg4 (Simple Profile) (FMP4 / 0x34504D46), yuv420p, 1920x818 [SAR 1:1 DAR 960:409], 1592 kb/s, 24 fps, 24 tbr, 24 tbn
    Stream #0:1: Audio: ac3 ([0] [0][0] / 0x2000), 48000 Hz, 5.1(side), fltp, 640 kb/s

Como puede observarse, los subtítulos también se pierden. ¿La razón? la misma de antes, AVI no ofrece soporte directo a los subtítulos, así que simplemente desaparecen.

Extrayendo audio
--------------------

Recordemos que pasa cuando examinamos nuestro fichero original:

    ffmpeg -i Sintel.2010.1080p.mkv

Obteníamos esto:

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

Como vemos, el audio está en formato ac3. Si deseásemos extraer el audio de esta película podríamos hacer esto:

1. ``ffmpeg -i Sintel.2010.1080p.mkv AudioSintel.mp3``
2. ``ffmpeg -i Sintel.2010.1080p.mkv AudioSintel.ac3``
3. ``ffmpeg -i Sintel.2010.1080p.mkv -acodec copy AudioSintel.ac3``


¿Qué harán estos comandos?

1. ``ffmpeg -i Sintel.2010.1080p.mkv AudioSintel.mp3`` extrae el audio de la película y lo guarda en mp3. Sin embargo, como el audio original de la película está en ac3 **habrá una recodificación**. 
2. ``ffmpeg -i Sintel.2010.1080p.mkv AudioSintel.ac3`` hace lo mismo, pero ``ffmpeg`` puede deducir viendo la extension ``.ac3`` que se desea guardar en ese formato. En realidad el  audio original estaba en ese formato sin embargo al no especificar el codec ``ffmpeg`` extrae el audio y lo recodifica, lo que es mas lento que el comando siguiente.
3. ``ffmpeg -i Sintel.2010.1080p.mkv -acodec copy AudioSintel.ac3`` extrae el audio y recalca que no queremos usar ningún codec, sino que queremos usar exactamente el mismo con el que se codificó el audio en la película. Esto hará la extracción del audio mucho más rápida.

Modificando el tamaño de los vídeos
--------------------------------------

En nuestro vídeo original la resolución era 1920x818. Es decir, 1920 píxeles en horizontal y 818 en vertical. Sin embargo, ¿qué es la resolución?. En pocas palabras la resolución es una forma de indicar el tamaño del vídeo y si hablamos de dispositivos como monitores o televisiones la resolución indica cuantos puntos se pueden mostrar. Cuanto mayor sea la resolución de un monitor, mayor precisión de imagen se podrá mostrar. Por tanto, un vídeo grabado a una resolución muy alta puede verse con un nivel de detalle mucho mayor, lo que llamamos una "mejor definición".

Todo esto implica lo siguiente:

* Si tenemos un vídeo con una resolución muy alta (p.ej 1920x818) y lo mostramos en un monitor que como mucho puede mostrar 1024x768 entonces no solo no podremos apreciar toda la calidad del vídeo original, sino que también estamos gastando mucho espacio en disco para un archivo del cual no podremos apreciar toda su calidad.

* Si tenemos un vídeo con una resolución muy baja (por ejemplo 720x576) y lo mostramos en una televisión con una resolución muy alta (3840x2160) estaremos desaprovechando la calidad de la televisión y quizá nos interese conseguir el mísmo vídeo a una resolución mayor.


Teniendo esto en mente, es posible que nos interese modificar el tamño de nuestro vídeo original de 1920x818. Podríamos tener varios motivos:

* Que este archivo sea demasiado grande para mostrarlo por ejemplo en una tablet antigua que quizá tenga una resolución pequeña.
* Necesitamos ahorrar espacio en disco.
* Nos da igual ver el vídeo aunque sea en una calidad ligeramente menor.

Usando ``ffmpeg`` podemos modificar el tamaño del vídeo usando una herramienta llamada "filtros". El ejecutable de ``ffmpeg`` incluye una amplia variedad de filtros de vídeo y de audio que permiten hacer muchas operaciones. Para acceder a un filtro de vídeo usaremos la opción ``-vf <nombre_del_filtro> <opciones_del_filtro>``. Probemos a ejecutar esto:

    ffmpeg -i Sintel.2010.1080p.mkv -vf scale=1280:720 -acodec copy Sintel.HD.mkv

Pasado un rato, ``ffmpeg`` habrá modificado el tamaño del vídeo y ahora tendrá la resolución indicada. Podemos comprobarlo con esto:


    ffmpeg -i Sintel.HD.mkv

Y veremos que efectivamente la nueva resolución es de 1280x720:

    Stream #0:0(eng): Video: h264 (High), yuv420p(tv, bt709/unknown/unknown, progressive), 1280x720 [SAR 540:409 DAR 960:409], 24 fps, 24 tbr, 1k tbn
        Metadata:
        ENCODER         : Lavc59.14.100 libx264
        DURATION        : 00:14:48.000000000
    Stream #0:1(eng): Audio: ac3, 48000 Hz, 5.1(side), fltp, 640 kb/s
        Metadata:
        title           : AC3 5.1 @ 640 Kbps
        DURATION        : 00:14:48.032000000
    Stream #0:2(ger): Subtitle: ass
        Metadata:
        ENCODER         : Lavc59.14.100 ssa
        DURATION        : 00:10:29.800000000

Si abrimos el vídeo en un ordenador normal es bastante probable que no lleguemos a notar la pérdida de calidad. Si ademas examinamos el tamaño de los archivo veremos una diferencia apreciable:

    1.172.428.172   Sintel.2010.1080p.mkv
      263.532.228   Sintel.HD.mkv

Resoluciones estándar
----------------------

A día de hoy (Diciembre de 2021) hay unas cuantas resoluciones que son bastante típicas y con nombres bastante conocidos:

| Nombre comercial | Significado              | Resolución |
|------------------|--------------------------|------------|
| UHD 8K           | Ultra High Definition 8K | 7680x4320  |
| UHD              | Ultra High Definition    | 3840x2160  |
| QHD              | Quad High Definition     | 2560x1440  |
| Full HD          | Full High Definition     | 1920x1080  |
| HD               | High Definition          | 1280x720   |

Una pregunta que cabría hacerse es ¿por qué la película que estamos manejando todo el tiempo no tiene ninguna de estas resoluciones? La respuesta es que aunque en un DVD o un BluRay nos vamos a encontrar estas resoluciones, con los vídeos creados en Internet puede ocurrir que los creadores decidan usar distintas proporciones o **aspect ratios** como se conocen en inglés.

Durante mucho tiempo, las TV normales tenían una proporción de 4:3. Es decir, podíamos tener una TV más grande o más pequeña pero si medíamos el ancho y el alto, la proporción siempre era de 4:3. Más adelante, las TV adoptaron una *ratio* de 16:9 para que la experiencia fuera más similar al cine (que nunca usó 4:3). Por desgracia, incluso en el cine no hay unanimidad sobre la proporción a usar. Así, podemos encontrar películas comerciales rodadas en proporciones de 2.35:1, de 2.59:1, de 1.85:1, 2.20:1 y otras. Si examinamos la resolución de la película original (1920x818) veremos que la proporción es 2.35:1 (2.3471:1 para ser exactos).

Los problemas con el escalado
--------------------------------

Si nuestra película original tenía una resolución de 1920x818 despues tiene una resolución de 1280x720 entonces **hemos cambiado la proporción de la película**. Efectivamente, la *aspect ratio* antes era de 2.35:1 (que sale de dividir 1920/818) y despues es de 1.77:1 (o lo que es lo mismo de 16:9). No siempre se nota, así que haremos un nuevo reescalado y modificaremos mucho el tamaño original y lo dejaremos por ejemplo en 8:1 (por ejemplo usando una resolución final de 800x100):

    ffmpeg -i Sintel.2010.1080p.mkv -vf scale=800:100 -acodec copy Sintel-800x100.mkv

Observaremos que la película ha perdido demasiada calidad con respecto al original. De hecho, es muy posible que se aprecie el pixelado al haber modificado demasiado la resolución vertical.


Una equivocación común es hacer lo siguiente:

    ffmpeg -i Sintel.2010.1080p.mkv -vf scale=7680:4320 -acodec copy Sintel-UHD.mkv


El error es que nuestra película tiene una resolución de 1920x818 pero *no podemos ampliarla a una resolución mayor.* El programa puede modificar el tamaño del vídeo original pero básicamente **le estamos pidiendo que se invente píxeles.** El archivo final tendrá la anchura y altura pedidos pero muy seguramente no la calidad esperada.


Segmentos de tiempo
-----------------------------

Es posible cortar un trozo de vídeo usando la línea de comandos. Para ello se deben tener en cuenta tres cosas:

* Los indicadores de tiempo deben escribirse SIEMPRE ANTES del fichero de entrada.
* Para indicar el punto de comienzo se usa algo como ``-ss 1:21:38.300`` que significa comenzar a cortar a partir de la hora 1, 21 minutos, 38 segundos con 300 centésimas. Si no indicamos nada más el programa cortará desde ese punto hasta el final.
* Se puede indicar un punto de final usando ``-to 1:23:41.200`` que en este caso indicaría llegar hasta 1 hora, 23 minutos, 41 segundos con 200 centésimas.

Probemos a extraer un trozo de vídeo entre dos tramos de tiempo:

    ffmpeg -ss 0:02:00 -to 0:03:30 -i Sintel.2010.1080p.mkv -acodec copy -vcodec copy Sintel-segmento.mkv

Esto cortará el vídeo empezando en el minuto 2 y llegando exactamente hasta el minuto 3:30, es decir, que el vídeo final durará 1 minuto y 30 segundos.


En ocasiones, el vídeo tendrá la duración correcta pero algunos programas de reproducción y/o televisiones informarán de una duración equivocada. Esto se debe a que algunos vídeos (como el que estamos usando de ejemplo) **incluyen información sobre capítulos y/o subtítulos** que al ser incluidos en el fichero final "confunden" al reproductor. El problema se debe a que el flujo de vídeo tiene una duración pero el flujo de subtítulos tiene una distinta. Esto puede ser difícil de resolver y el propio software indica que no hay una solución factible, ya que se depende de demasiadas variables.

Unir ficheros
---------------

Igual que podemos partir un vídeo en trozos, podemos unir varios vídeos en uno. En realidad, el concepto de "unir" puede ser bastante antiguo según el usuario. El siguiente texto está tomado literalmente de la [documentación de FFMPEG sobre "unión" de vídeos](https://ffmpeg.org/faq.html#How-can-I-join-video-files_003f):

>Unir puede significar:
>
>* Poner un fichero detrás de otro. En ``ffmpeg`` esto se llama "concatenación".
>* Ponerlos en el mismo fichero y que el usuario elija. A esto se llama "multiplexación".
>* En el caso de audio podría significar unir dos flujos de audio en uno solo, por ejemplo unir dos canales de audio mono en una sola pista estéreo. A esto se le llama "mezclar" (merge en el original).
>* De nuevo en el caso de audio podría interesar hacer una "remezcla" (mix en el original). Es decir, modificar el audio de manera que por ejemplo un canal se oiga más alto que el otro.
>* En vídeo podría interesar hacer una "superposición" de manera que ambos vídeos se vean a la vez.

Para hacer algunas de estas operaciones y otras más interesantes, antes habrá que introducir un concepto nuevo: los filtros.


Filtros
------------------

En pocas palabras un filtro es una operación que puede tener **una o varias entradas** y a la vez **una o varias salidas**. El siguiente es un ejemplo de diagrama extraído de [la documentación sobre filtros de ``ffmpeg``](https://ffmpeg.org/ffmpeg-filters.html#Filtering-Introduction)

                [main]
    input --> split ---------------------> overlay --> output
                |                             ^
                |[tmp]                  [flip]|
                +-----> crop --> vflip -------+


En él se puede ver lo siguiente:

1. Se toma un fichero de entrada.
2. Se lleva a un *filtro* llamado ``split`` el cual va a producir dos salidas. Una que el usuario ha llamado "main" y otra que ha llamado "tmp".
3. El flujo llamado "tmp" (podremos poner los nombres que queramos) se va a enviar a otros dos filtros. Uno de los filtros es ``crop`` y el otro ``flip``. Cuando terminemos con este último filtro tendremos una salida de vídeo llamada "flip".
4. Por último, el flujo llamado "main" y el flujo llamado "flip" se unen en uno solo que se enviará a la salida.

En pocas palabras, este diagrama representa como un vídeo se divide en dos mitades. La mitad de arriba no se toca, y la de abajo será un reflejo de la de arriba. Lo probamos con esto:

    ffmpeg -i Sintel-segmento.mkv -vf "split [main][tmp]; [tmp] crop=iw:ih/2:0:0, vflip [flip]; [main][flip] overlay=0:H/2" Efecto-Espejo.mkv

Esto cogerá nuestro segmento de vídeo recortado en la sección anterior y "construirá" un vídeo con el efecto espejo.

En realidad este truco aplica varios filtros seguidos:

* El filtro ``split`` coge un flujo de vídeo y lo duplica en dos flujos exactamente iguales.
* El filtro ``crop`` que "recorta" un vídeo usando cuatro parámetros: anchura, altura, x inicial (0 es la izquierda del todo) e y (0 es arriba del todo). Así, como vemos, el flujo llamado "tmp" (que es como el vídeo inicial) se verá recortado dejando solo la mitad superior del vídeo.
* El filtro ``vflip`` que invierte un vídeo en vertical.

Así, concatenando distintos filtros se pueden conseguir efectos bastante espectaculares. Un ejemplo más simple sería este: tomemos nuestro vídeo inicial y dividámoslo en dos trozos. En uno irá la mitad superior del vídeo y en otro la mitad inferior:

    ffmpeg -i Sintel-segmento.mkv -filter_complex "split [arriba][abajo]; [arriba] crop=iw:ih/2:0:0 [salida1]; [abajo] crop=iw:ih/2:0:ih/2[salida2]"  -map "[salida1]" arriba.mkv -map "[salida2]" abajo.mkv

Como este vídeo tiene varias salidas *necesitamos* usar la opción** ``filter_complex``. El uso de todos los filtros es algo lo bastante complejo para quedar dentro de un tutorial. Aún así comentamos a continuación algunos filtros de vídeo.

### Filtro ``crop``



Aunque ya lo hemos visto, este filtro permite cortar zonas del vídeo. El filtro necesita que pasemos cuatro datos:

* Anchura de la región a cortar.
* Altura de la región.
* Coordenada X inicial. El punto x=0 está en la parte izquierda del vídeo.
* Coordenada Y final. El punto y=0 está en la parte superior del vídeo.

Así, supongamos que tenemos un vídeo de 1920x808 y que deseamos cortar el lado izquierdo del vídeo. Usaríamos algo como esto:
    ffmpeg -i Sintel-segmento.mkv -vf "crop=960:808:0:0" -acodec copy Sintel-trozo-izquierdo.mkv

### Filtros ``hflip`` y ``vflip``


Giran el vídeo horizontal y verticalmente. En el caso del giro horizontal veremos el vídeo como si hubiera un espejo. En el caso del giro vertical, veremos el vídeo cabeza abajo. Por ejemplo, podemos hacer el giro vertical con esto:

    ffmpeg -i Sintel-segmento.mkv -vf "vflip" -acodec copy Sintel-girado.mkv


### Aplicando varios filtros a la vez

Se pueden aplicar varios filtros de la manera siguiente:

*  Usaremos  la opción ``-filter_complex``.
*  Escribiremos los nombres de los filtros con punto y coma.
*  Los filtros producen salidas y esas salidas deberán llevar un nombre que pondremos entre corchetes.
* Las salidas de un filtro se pueden pasar al siguiente usando el nombre definido antes.


Por ejemplo, supongamos que queremos cortar un cuadrado de vídeo de 400x400 que empiece en las coordenadas (0,0) y a la vez poner el vídeo cabeza abajo:

    ffmpeg -i Sintel-segmento.mkv -vf "crop=400:400:320:3200 [trozo];[trozo] vflip" -acodec copy Sintel-cortado-y-girado.mkv

Aquí vemos que:

1. Se aplica primero el filtro crop, que hace el corte y el vídeo cortado se convierte en un flujo cuyo nombre es "trozo".
2. El flujo llamado "trozo" se le pasa al filtro "vflip" que genera el vídeo final.

### Filtro subtitles

Este filtro permite insertar textos en el vídeo **dibujándolos encima de la imagen**. Por lo tanto **no se pueden borrar, ni deshabilitar**. 

Una posibilidad es coger los propios subtítulos que nuestro vídeo de ejemplo incluye. Podemos conseguir esto con:

    ffmpeg -ss 0:01:00 -to 0:01:30 -i Sintel.2010.1080p.mkv  -acodec copy -vf "subtitles=Sintel.2010.1080p.mkv" -acodec copy Sintel-segmento-con-subs-integrados.mkv

Esto coge la película original y procesa solo 30 segundos de película (así tardamos menos), deja el audio como estaba (ahorrando más tiempo aún) y dibuja los subtítulos tomando la primera pista de subtítulos (que en nuestro caso inserta los subtítulos en inglés)


Otra posibilidad es crear nuestro propio fichero de subtítulos para, por ejemplo, poner algún pequeño letrero al principio. Crearemos un pequeño fichero en el que ir indicando los subtítulos que deben aparecer y cuando deben aparecer.

    1
    00:00:05,000 --> 00:00:20,000
    Ejemplo de edición de vídeo.
    Subtítulo integrado

En este formato se van poniendo los subtítulos.
* Primero se indica el número de subtítulo.
* Despues en otra línea se indica en qué momento empieza y en qué momento acaba. Ambos momentos se separan con una flecha
* Despues podemos poner una o dos líneas que aparecerán en el vídeo en el momento indicado.