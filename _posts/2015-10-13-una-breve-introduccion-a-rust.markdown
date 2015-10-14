---
layout: post
title: "Una breve introducción a Rust"
date:  2015-10-14 18:00:00
categories: editoriales
author: cristian
language: spanish
---

Desde la semana pasada estoy aprendiendo y empezando a usar el lenguaje de
programación [Rust](https://www.rust-lang.org/) de [Mozilla](https://www.mozilla.org/en-US/)
el cual tiene varias particularidades interesantes que me gustaría compartir
con ustedes.

A modo de introducción, Rust es formalmente un lenguaje de sistemas (o sea que
está disañado para programar sistemas operativos completos en él) como también
lo son C o C++. Una de las primeras curiosidades que vale la pena resaltar es que,
si bien te permite hacer un [uso completo del sistema](https://doc.rust-lang.org/book/the-stack-and-the-heap.html)
pudiendo por ejemplo, decidir donde alocar variables (en la pila, stack, o en el cúmulo, heap), una de sus principales características es la de ofrecer total control de la máquina sin
sacrificar la [seguridad](https://www.youtube.com/watch?v=d1uraoHM8Gg) (en términos de manejo de memoria) ni la eficiencia en
el uso de sus recursos.

Muchos de nosotros estamos acostumbrados a programar en lenguajes dinámicamente
tipados como Ruby o Javascript; Rust no es uno de ellos. Sin embargo, su sistema
de tipos es justmente otro de sus puntos fuertes, ya que gracias a su
[inferencia](http://rustbyexample.com/cast/inference.html) de tipos de datos,
el escribir programas en Rust no es tan verborrágico como otros lenguajes.
Por ejemplo, si escribimos:

```rust
let respuesta = 42; // el tipo inferido es i32.
```

verán como no es necesario explicitar el tipo de dato con el que estamos trabajando.
Como todo lenguaje moderno, Rust tiene soporte para tipos [genéricos](https://doc.rust-lang.org/book/generics.html) al estilo C++ o Java, así como también existe soporte para
interfaces y clases abstractas llamadas [traits](https://doc.rust-lang.org/book/traits.html)
en su terminología.

Otro punto interesante son los paradigmas de programación soportados por el
lenguaje. Rust nos permite programar de manera imperativa/estructurada usando
[procedimientos](http://rustbyexample.com/fn.html) de la misma manera que lo hacen
lenguajes de scripting.  De modo similar, Rust también nos permite escribir
programas con un estilo más funcional y de hecho, ese es el estilo recomendado
ya que hay varias de las contrucciones del lenguaje que así lo suguieren.
Ejemplos de estas características son:

* las [variables inmutables](http://rustbyexample.com/variable_bindings/mut.html) por defecto
* las [funciones anónimas](https://doc.rust-lang.org/book/closures.html) (closures)
* las funciones de primer órden (HOF o Higher Order Functions)
* el [pattern matching](https://doc.rust-lang.org/book/patterns.html)

Para los interesados y ansiosos es bueno aclarar que Rust también soporta el
paradigma de orientación a objetos mediante el uso de [estructuras](http://rustbyexample.com/custom_types/structs.html) (structs).
Asimismo, podemos implementar llamadas a métodos polimórficos (a.k.a. dynamic
dispatch) mediate [trait objects](https://doc.rust-lang.org/book/trait-objects.html).

En cuanto a herramientas propias del lenguaje (build tools) Rust usa [Cargo](https://doc.rust-lang.org/book/hello-cargo.html)
para el empaquetado de binarios/bibliotecas y gestión de dependecias. Los rubystas
podemos ver a Cargo como una mezcla entre el venerable Rake y Bundler. Hablando
de dependencias, [crates.io](https://crates.io/) es el equivalente a rubygems,
donde los rustaceans (programadores Rust) publican sus bibliotecas de software.

Antes de despedirme les quería comentar dos cosas:

1. Estoy interesado en desarrollar y promover el uso del lenguaje en América
Latina. Si eso es algo que también les interesa no dejen contactarme por
[Twitter](https://twitter.com/cristianrasch).

2. Aprovecho también para dejarles un par de recursos para que puedan seguir
aprendiendo sobre Rust si les interesa:

* [Steve Klabnik](https://www.youtube.com/watch?v=_-fweBvtifA) sobre porque usar Rust (en inglés)
* El [libro](https://doc.rust-lang.org/book/) de Rust
* [Ejemplos prácticos](http://rustbyexample.com/) mostrando varias características y
mejores prácticas del lenguaje
* `#rust` en irc.mozilla.org

Gracias por leer hasta acá y hasta la próxima :)
