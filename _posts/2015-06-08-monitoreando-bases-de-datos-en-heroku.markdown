---
layout: post
title: "Monitoreando bases de datos en Heroku"
date:  2015-06-08 14:30:00
categories: heroku postgres protip
author: delucas
language: spanish
---

Hoy día es muy fácil tener aplicaciones en [Heroku][heroku], y muchos de nosotros utilizamos el plan gratuito para comenzar con algunos experimentos.  Ellos avisan que tenemos una disponibilidad de 10K registros en la base de datos, pero usualmente nos olvidamos de revisar esto. También nos recuerdan la situación cuando estamos próximos a quedarnos sin espacio, para que cambiemos a un [plan más adecuado][planes] a nuestras necesidades.

Si queremos anticiparnos, y verificar cuántos registros llevamos usados podemos acceder a la base de nuestra aplicación y ejecutar la siguiente consulta:

    $ heroku pg:psql --app gulliver

    gulliver::DATABASE=> select sum(n_live_tup) as total from pg_stat_user_tables;
     total 
    -------
      8498
    (1 row)

Si, en cambio, queremos un detalle por tabla, podemos ejecutar una versión más completa del script:

    gulliver::DATABASE=> select schemaname,relname,n_live_tup 
    gulliver::DATABASE->   from pg_stat_user_tables 
    gulliver::DATABASE->   order by n_live_tup desc;

[heroku]: http://www.heroku.com
[planes]: https://elements.heroku.com/addons/heroku-postgresql#addon-plans