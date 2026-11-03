---
title: Clustering Jerarquico
date: 2026-11-03 00:00 -0300
categories:
  - Clustering
tags:
  - Clustering
  - Unsupervised
math: true
---
## Introduccion al clusting jerarquico

En otro [articulo](https://lucio-raimondi-pazos.github.io/posts/clustering/) introdujimos el concepto de clustering con KMeans y Fuzzy C Means. Estos dos metodos de clustering son los llamados _clustering por particion_. En este articulo nos vamos a enfocar en otro tipo, el _clustering jerarquico_. Este tipo de clustering es util para, como dice el nombre, cuando los datos tienen algun tipo de jerarquia intrinseca.
Existen dos metodos de clustering jerarquico: aglomerativo, que comienza con cada punto siendo un cluster individual, y divisivo, que comienza con todos los puntos dentro del mismo cluster y los va dividiendo a medida que avanza el algoritmo. Por cuestiones de orden de complejidad (¿recuerdan la Big-O notation, no?) vamos a analizar solo el caso del aglomerativo. El objetivo final de estos algoritmos es encontrar el dendograma, una herramienta visual que permite al usuario ver los clusters, y permite encontrar un punto de detención que sea coherente con el problema. Cabe destacar, que a diferencia de los métodos previamente vistos, el clustering jerarquico no requiere indicar un número de clusters a priori. 

## Clustering aglomerativo

El algoritmo es el siguiente: 
1. Se representa cada punto como un cluster.
2. Se calcula la matriz de cercania.
3. Juntar a los clusters con la distancia minima entre ellos.
4. Si queda un solo cluster, el dendograma esta completo. Caso contrario se repite el paso 2 y el paso 3.

Nuevamente aparece el concepto de distancia para la matriz de cercanía. En este caso, usaremos de nuevo la distancia euclidiana como para el caso de KMeans y Fuzzy C Means. Para mostrar un ejemplo, observen el siguiente conjunto de datos:
![[Pasted image 20261103003744.png]]

![[Pasted image 20261103003854.png]]

La forma de leer el grafico es la siguiente: en el primer paso, se unen D y C como un solo cluster, generando el cuadrado naranja. Luego, se unen B y A, generando el rectangulo verde. A esa union, ese nuevo cluster, se une el cluster de E. Finalmente, al quedar solo dos clusters, se unen los dos generando la raiz del arbol. El usuario entonces puede elegir donde "cortar" el arbol para obtener los clusters que les convenga. Por ejemplo, si cortamos en la linea azul

![[Pasted image 20261103004114.png]]

Obtendremos solamente los clusters DC y AB, y el cluster E por su lado. Como siempre, dependiendo de la naturaleza del problema y del contexto dado, nos convendra algun corte u otro. 

## Criticas
Una de las críticas principales a este método es su sensibilidad a outliers y ruido. Cuando un elemento se asigna a un cluster, no se lo vuelve a tener en cuenta, eliminando la posiblidad de recalcular un cluster si el agrupamiento inicial es incorrecto. Finalmente, se entiende que el orden de complejidad es de al menos $O(N^2)$, lo que no lo hace muy eficiente. Se han hecho intentos de corregir estos problemas mediante los siguientes metodos, que solo serán mencionados para el lector curioso que decida investigar más por su cuenta. 

1. BIRCH (Zhang, et al.,1996)
2. CURE (Guha, et al., 1998)
3. ROCK (Guha, et al., 2000)
4. Chameleon (Karypis et al., 1999)

## Links
Se deja el link al Google Colab con el codigo utilizado para generar las visualizaciones: [Link a Google Colab](https://colab.research.google.com/drive/1DRayEKSgI05NDUJEvXoeKe0I30rWrGZi?usp=sharing)
## Fuentes

La fuente principal para este articulo fue "Clustering", de Rui Xu y Donald C. Wunsch. 