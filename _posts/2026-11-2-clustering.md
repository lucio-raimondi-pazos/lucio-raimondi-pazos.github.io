---
title: Introduccion a la teoria de Clustering
date: 2026-11-02 01:25:00 -0300
categories:
  - Clustering
tags:
  - Clustering
  - Unsupervised
math: true
---
# Introduccion

El siguiente texto sirve como introducción a la teoría de clustering, un metodo de aprendizaje no supervisado. Se analiza las motivaciones para querer agrupar cosas, y se avanza sobre los temas clasicos de clustering, como K-Means, Fuzzy C Means, y luego se introducen temas mas avanzados como _Kernel Clustering_ y _Neural Network Based Clustering_. 

# Parte 1: ¿Para qué agrupar?

Resulta interesante la idea de agrupamiento desde un punto de vista conceptual. Los seres humanos tienen la tendencia a agrupar cosas en el día a día, evidenciado por nuestra gran capacidad de detección de patrones. Como va a ser moneda corriente a lo largo de éstas entradas, el clustering se inspira precisamente en nuestra capacidad de construir grupos abstractos de cosas basados en algún tipo de característica común. Especialmente importante resultan estas habilidades en un mundo lleno de datos, que se pueden volver imposible de manejar a mano cuando su magnitud aumenta. Este problema vienen a solucionar los algoritmos de clustering. 
Nos hacemos la pregunta motivadora: **¿puede una computadora identificar patrones?**

_Nota:  Nada de este articulo contiene trabajo original. Se citan de fuentes las clases y diapositivas de Inteligencia Computacional de la Universidad Nacional de Mar del Plata, Facultad de Ingeneria, y el libro "Clustering" de Rui Xu y Donald C. Wunsch. Para quien quiera ahondar mas en estos temas, se recomienda fuertemente su lectura._ 
# Parte 2: Clusters

Partamos de una base, ¿qué es un cluster? Las definiciones varian, pero propongo las siguientes como para tener una base común desde la cual avanzar:

- Un cluster es un conjunto de entidades que son parecidas, y entidades de diferentes clusters que no son parecidas.
- Un cluster es un agregado de puntos en el espacio de prueba de forma tal que la distancia entre dos puntos cualesquiera dentro de un cluster es menor que la distancia entre cualquier punto en el cluster a uno no en él. 

Las definiciones pueden ser confusas a primera mirada, pero van a esclarecerse a medida que avancemos con este articulo. Especialmente quiero hacer énfasis en la segunda definición, porque ya nos habla de un concepto importante: la distancia intracluster y la distancia intercluster (o, en términos más académicos, la homogeneidad interna y la separacion externa). Acá debería aparecer una pregunta importante: ¿qué significa que un punto esté más cercano a otro? O dicho en otras palabras, ¿qué implica que dos puntos sean parecidos? La respuesta a esto no es para nada trivial, y va a ser de fundamental importancia en los diferentes algoritmos y ejemplos que veamos.

## Parte 2.1. La importancia de las caracteristicas

Siendo este el primer artículo en el blog, me parece importante hacer un paréntesis respecto a clarificar de que hablamos cuando hablamos de características, y por qué son tan importantes. 
Las características o atributos de un conjunto de datos son representaciones de algun concepto o cosa física del mundo real, resumido en un numero. Por ejemplo, podemos considerar un conjunto de datos de flores, donde el largo de sus petalos es una característica. De ésta forma, le estamos dando cierta "personalidad" a los datos en función de sus atributos, y también le estamos dando forma al problema al elegir que atributos usar. Resulta importante entonces saber que atributos usar dentro de un conjunto de datos. Si queremos agrupar flores según su tamaño, ¿nos importa su color? La respuesta a esto tampoco es trivial, pues el color puede ser un factor funcional al tamaño de una flor (disculpen al autor por cualquier información erronea debido a su falta de conocimiento de flora). El punto de esto es que la elección de características es muy importante, dado que su correcta formación puede darnos el agrupamiento clave que estabamos buscando, y nos permite omitir información superflua.  También, visto desde otra perspectiva, al utilizar características que a priori parecen no tener relación, pueden aparecer relaciones ocultas que el usuario no encontró a ojo desnudo.  Volviendo a la idea del largo de los petalos de una flor, un lector despierto puede haber notado que presentamos implicitamente la primera ida de _parecido_. En mi conjunto de datos las flores se agrupan por tamaño de petalo. ¿Por qué? Porque quiero, y porque me sirve para el dominio de mi problema. El algoritmo de clustering entonces, debería, agrupar por estas características. No perdamos de vista que es posible acotar el problema hasta llegar a algo que nos sirva.