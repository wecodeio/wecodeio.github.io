---
layout: post
title: "Descifrando tu resumen de Visa"
date:  2015-07-31 11:30:00
categories: protip cli
author: delucas
language: spanish
---

Si tenés Visa, seguramente una vez por mes te llega por correo electrónico el resumen de cuenta. Este archivo está cifrado, y la contraseña suele ser muy simple... pero es tedioso tener que tipearla cada vez que querés abrir el archivo.  
Hace unos meses [Joaquín][wacko] me mostró lo que él hace con esos archivos (que es muy parecido a lo que quiero compartir aquí). Él lo descomprime y lo copia en su carpeta de resúmenes en [Dropbox][dropbox].

Yo lo voy a hacer por pasos. Por empezar, necesitamos instalar una herramienta que pueda hacer el descifrado del archivo, y `qpdf` resultó muy útil en Linux. El comando quedaría algo así:

    qpdf --password=tu_password --decrypt /full/path/to/file/0451533920.02.23-07-15.pdf VISA-23-07-15.pdf

Y eso genera, en este caso, el archivo `VISA-23-07-15.pdf`. Este último puede accederse libremente :smile:

Si quisiéramos ir un paso más allá, habría que moverlo a la carpeta de Dropbox. Una simple línea:

    mv VISA-23-07-15.pdf /full/path/to/Dropbox/VISA/VISA-23-07-15.pdf

¡Y listo!... ¿listo? Nah, esto fue muy simple. Vamos a hacer algo más: no tipear el comando **cada vez** reemplazando el nombre del archivo. Esto sería así:

    #!/bin/bash

    INPUT=`ls -lt | grep pdf | head -n 1 | awk '{print $9}'`
    OUTPUT=VISA-`date +"%Y%m%d"`
    echo $INPUT
    echo $OUTPUT
    qpdf --password=$1 --decrypt `pwd`/$INPUT $OUTPUT
    mv $OUTPUT $HOME/Dropbox
    rm $INPUT

De paso, pasamos el password por línea de comandos para que este script [se pueda compartir][gist].

    ./decrypt_visa.sh mi_password

Sin embargo, en nuestro caso, al descargarlo o hacer una versión mejorada... podemos utilizar la contraseña en texto plano. Después de todo... está dentro de nuestra computadora :wink:

¿Qué te pareció este `protip`? ¿Cómo se te ocurre mejorarlo? ¡Dejame tu comentario!

[wacko]: http://www.twitter.com/joaquinvicente
[dropbox]: http://www.dropbox.com
[gist]: https://gist.github.com/delucas/b669f527094815b6f114