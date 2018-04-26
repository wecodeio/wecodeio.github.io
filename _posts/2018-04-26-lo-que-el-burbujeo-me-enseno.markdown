---
layout: post
title: "Lo que el burbujeo me enseñó"
date:  2018-04-26 12:00:00
categories: investigacion java
author: delucas
language: spanish
---

No dejo de alegrarme cuando creo que entiendo un tema y de repente aparece una pieza de información adicional que me hace replantearme los preconceptos. Es una situación ideal para repensar algo que, de otra manera, hubiera dado por sentado y nunca me hubiera detenido a considerar.

## Contexto

Esta vez viene de la mano de un pequeño experimento que hacemos con nuestros alumnos de Programación Avanzada, en la Universidad Nacional de La Matanza. La clase correspondía a métodos clásicos de ordenamiento, y estábamos analizando el rendimiento de cada uno de ellos.
La forma más simple de hacerlo es generar _arrays_ de sucesivamente más elementos, y tomar tiempos de ejecución. Las tres configuraciones clásicas son las de _array_ aleatorio, _array_ ordenado y _array_ invertido.

## El algoritmo

El algoritmo de [ordenamiento por burbujeo](https://en.wikipedia.org/wiki/Bubble_sort) tiene una [complejidad computacional](http://bigocheatsheet.com) bien conocida: `O(n²)` comparaciones, y `O(n²)` intercambios.
El algoritmo básico no puede evitar comparaciones, ya que son parte fundamental de su código. Por lo tanto, siempre compara una cantidad constante de n² veces. Sin embargo, en los tres casos clásicos varía completamente la cantidad de intercambios: si el _array_ viniera ordenado, no necesita hacer intercambios; si viniera aleatoriamente ordenado, haría un promedio de (n²)/2 intercambios; y en el caso invertido haría todos los intercambios posibles: n².  

## Las mediciones

Armé un pequeñísimo código para tomar mediciones de tiempos, comparaciones e intercambios para una cantidad logarítmicamente ascendente de elementos. Aquí los resultados para cada una de las configuraciones:

### Configuración aleatoria

| N      | Tiempo(ms) | Comparaciones | Intercambios | 
|--------|------------|---------------|--------------| 
| 100    | 0          | 27225         | 14163        | 
| 1000   | 2          | 2796750       | 1389552      | 
| 10000  | 122        | 280017000     | 140293851    | 
| 100000 | 17061      | 28004719500   | 13988877980  |

### Configuración ordenada

| N      | Tiempo(ms) | Comparaciones | Intercambios | 
|--------|------------|---------------|--------------| 
| 100    | 0          | 27225         | 0            | 
| 1000   | 3          | 2796750       | 0            | 
| 10000  | 35         | 280017000     | 0            | 
| 100000 | 3056       | 28004719500   | 0            | 

### Configuración invertida

| N      | Tiempo(ms) | Comparaciones | Intercambios | 
|--------|------------|---------------|--------------| 
| 100    | 0          | 27225         | 27225        | 
| 1000   | 1          | 2796750       | 2796750      | 
| 10000  | 79         | 280017000     | 280017000    | 
| 100000 | 7845       | 28004719500   | 28004719500  | 

## Análisis

Y aquí es que los números se contradicen (en principio). Podemos ver que la cantidad de intercambios sigue la tendencia que marca la teoría de complejidad computacional:

<div id="graph-swaps" class="graph"></div>

Sin embargo, los tiempos son los que llevan la contra:

<div id="graph-time" class="graph"></div>

¿Cómo puede ser que el caso extremo, en el que el _array_ se encuentra invertido, tome menos tiempo que el caso promedio, en el que el _array_ se encuentra en orden aleatorio? :see_no_evil:

## El concepto nuevo (para mí)

No les voy a mentir: cuando sucedió esto, yo no estaba en el aula cuando esto sucedió. Pero me enteré por el grupo de docentes, y quise investigar. Un estudiante nombró el concepto, y eso diparó las búsquedas: el _branch prediction unit_.

#### ¿Qué es la unidad de predicción de saltos _(branch prediction unit)_? 

> En arquitectura de computadoras, un predictor de ramas es un circuito digital que intenta adivinar por qué rama (por ejemplo, en una estructura **if-then-else**) ejecutará el código hasta que se conozca a ciencia cierta. Su propósito es el de mejorar el flujo de ejecución. Los predictores de rama desempeñan un rol crítico en conseguir un rendimiento alto y efectivo en muchas arquitecturas de microprocesador como la x86.  
> _Fuente: [Wikipedia](https://en.wikipedia.org/wiki/Branch_predictor)_

En mis propias palabras, y aplicado al caso: resulta que la predicción favorece la rama por la cual se realiza el intercambio en el algoritmo de burbujeo. Entonces, ahorra la comparación y ejecuta con una certeza del 100% de los casos en el escenario de _array_ invertido.  
En cambio, en el escenario de _array_ aleatorio, no puede realizar las predicciones, y se fuerza a evaluar las condiciones individualmente.

Algo que debo destacar, es que esta tecnología **no es nueva**, ni mucho menos. Hay primeros experimentos con la técnica que datan de los años '50.

Tendré que estar más atento a posibles oportunidades para desbancar preconceptos. ¡Resulta que ahora en el peor caso los algoritmos de ordenamiento funcionan muy bien!

> **Corolario:** Habrá que empezar a probar con algoritmos "casi invertidos", para ejercitar casos extremos.

<script language="javascript">
  function draw() {

	c3.generate({
		bindto: '#graph-swaps',
	    data: {
	        x: 'x',
	        columns: [
	            ['x', 100, 1000, 10000, 100000],
	            ['ordenado', 0, 0, 0, 0],
	            ['aleatorio', 14163, 1389552, 140293851, 13988877980],
	            ['invertido', 27225, 2796750, 280017000, 28004719500]
	        ],
	        type: 'spline'
	    },
	    axis: {
	        x: {
	            label: 'N',
	            tick: {
	            	count: 1
	            }
	        },
	        y: {
	            label: 'Intercambios'
	        }
	    }
	});

	c3.generate({
		bindto: '#graph-time',
	    data: {
	        x: 'x',
	        columns: [
	            ['x', 100, 1000, 10000, 100000],
	            ['ordenado', 0, 3, 35, 3056],
	            ['aleatorio', 0, 2, 122, 17061],
	            ['invertido', 0, 1, 79, 7845]
	        ],
	        type: 'spline'
	    },
	    axis: {
	        x: {
	            label: 'N',
	            tick: {
	            	count: 1
	            }
	        },
	        y: {
	            label: 'Tiempo (ms)'
	        }
	    }
	});

  }

 window.onload = draw;
</script>