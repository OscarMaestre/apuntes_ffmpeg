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

Ahora la conversión sí se producirá y será más rápida que la realizada inicialmente (que recordemos convirtió el vídeo de h264 a mpeg4 el audio de ac3 a mp3). Si consultamos los códecs usados en el fichero final con esto:

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


    ffmpeg -i Sintel.2010.1080p.mkv -vf scale=1280:720 -acodec copy Sintel.HD.mkv

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