---
title: "Redes Neuronales desde el principio: el Perceptron"
date: 2026-11-07 00:00 -0300
categories:
  - Neural Networks
tags:
  - machine_learning
  - supervised
math: true
---
## Parte 1. ¿Qué es una neurona?

Puede no resultar obvio que el primer paso a aprender qué es una red neuronal implica entender cómo nuestro propio cerebro funciona, pero es una pieza de información clave para desarrollar la intuición necesaria para entender el complejo mundo de redes neuronales, así que vamos a ello.

El cerebro consiste de un conjunto de muchísimas células nerviosas conectadas, llamadas neuronas. El cerebro humano incorpora casi 10 billones de neuronas y 60 trillones de conexiones, las llamadas sinapsis. Señales son propagadas de una neurona a otra mediante complejos procesos electroquímicos que generan un cambio en el potencial del cuerpo de la neurona. Una vez que este potencial supera un umbral, se dispara un impulso eléctrico, el potencial de acción, que es enviado a través del axón, una fibra larga que conecta neuronas entre sí. Un mecanismo clave para nuestrio estudio es la _plasticidad_ de las neurones, es decir, en respuesta a un patron de estimulacion, las neuronas muestran cambios a largo plazo en la fuerza de las conexiones. Incluso colecciones enteras de neuronas pueden migrar a otra parte del cerebro. 

## Parte 2. El perceptrón simple

¿Cómo podemos modelar el elemento más simple de una red neuronal? Mediante el **perceptrón simple**. Es la unidad básica de una red neuronal, y sienta las fundaciones necesarias para el análisis de redes neuronales más complejas a ser analizadas posteriormente.

![Perceptron](/assets/img/Pasted image 20261103010946.png)

El modelo es el mostrado en la imagen. Tenemos cierta $n$ cantidad de entradas, ponderadas cada una por pesos $w_{i}$. Estos pesos representan la 'fuerza' que tiene una entrada sobre la neurona. En 1943, Warren McCulloch y Walter Pitts propusieron una simple idea que es la base de las redes modernas: el perceptrón computa la suma ponderada de sus entradas; si el resultado es menor a un valor umbral $b$, la neurona no se activa (salida igual a $-1$). Si en cambio, la suma ponderada supera el umbral, la neurona se activa, indicando una salida de $1$. En otras palabras, la neurona usa la siguiente función de activación:

$$
X = \sum_{i=1}^{n} x_i w_i
$$

$$
Y = \begin{cases} 
+1 & \text{si } X \geqslant b \\ 
-1 & \text{si } X < b
\end{cases}
$$

donde $X$ es el resultado de la suma ponderada de las entradas, e $Y$ la salida. Este tipo de activación lleva el nombre de activación por signo, que lo que indica es que la neurona simplemente entrega un signo dependiendo de si supera el umbral o no. Otra manera de expresarlo sería como:

$$
Y = \text{sign}\left(\sum_{i = 1}^n x_{i}w_{i} - b\right)
$$

Existen otras funciones de activación, como la sigmoidea y la lineal. La eleccion de que funcion de activacion utilizar queda a criterio del usuario, que deberia saber cual es la que mas le conviene para un problema dado. 

Cabe destacar algo importante: las salidas, independientemente de la función de activación que usemos, son salidas binarias. Solamente pueden "distinguir" entre dos clases. Dependiendo de qué sea la salida, está en una clase o en la otra. Esto genera que el perceptrón simple separe el espacio del problema con un hiperplano (independientemente de la dimensionalidad del problema), lo que en consecuencia hace que solo sea útil en problemas donde los datos puedan ser **linealmente separables**.

### Parte 2.1. ¿Pero qué significa qué el perceptrón aprenda?

Es relevante hablar un poco de a que nos referimos cuando decimos que un perceptron "aprende". Como fue explicado, las neuronas del cerebro tienen la capacidad de reforzar ciertos vínculos con otras neuronas, su plasticidad. En neuronas artificiales ocurre algo muy similar. Tenemos nuestros parámetros modificables, los pesos, y queremos hacer que la neurona aprenda ciertos patrones. El aprendizaje ocurre precisamente cuando los pesos se modifican mediante algoritmos, como el que vamos a ver a continuación. Ésta noción es importante, porque representa la plasticidad de las neuronas artificiales. Si bien para redes neuronales más complejas el algoritmo de aprendizaje es más denso algoritmicamente, la idea es la misma. Reforzamos las conexiones que queremos y limitamos las que no. 
Pero, ¿qué es el aprendizaje en términos generales, o qué queremos lograr? Queremos lograr la capacidad de que reconozca patrones en los datos. Se lo entrena con un conjunto de datos conocidos, y se espera que frente a datos no conocidos pueda clasificarlos satisfactoriamente. Acá entra el concepto de un _algoritmo supervisado_, esto quiere decir que durante el entrenamiento, podemos ver si los resultados fueron correctos o no. El tener esa capacidad es lo que permite el aprendizaje, dado que si no clasifica correctamente, es momento de modificar nuestros parámetros para mejorar los resultados.

También cabe destacar, creo yo, el nivel de abstracción que estamos manejando. _No todos los pesos tienen una representación 1 a 1 con un concepto del mundo real_. ¿A qué me refiero con esto? Qué puede ser dificil interpretar que significa que la entrada 1 tenga un peso de 0.5. Quizás con perceptrones simples esto es más sencillo, pero cuando pasemos a redes multicapa, y ni hablar de modelos modernos como transformers, resulta prácticamente imposible interpretar que pasa en las capas ocultas (adelanto este término del próximo tema, no se asusten, ya va a llegar la explicación). A esto le llamamos _interpretabilidad_, que es un campo de estudio con relevancia actualmente. Pero me estoy yendo de tema, ¡la idea era hablar del perceptrón! Pero dado que este es el primer artículo en una serie, me pareció relevante dar este "primer" sobre aprendizaje. Espero que se puedan llevar una idea, un tanto abstracta, sobre el aprendizaje de redes neuronales artificiales.

Bien, con eso ya dicho, quienes no estén interesados en el detalle computacional del aprendizaje pueden detenerse acá. Es mi objetivo, que espero haber logrado, que se lleven una noción básica del aprendizaje en redes artificiales. Quienes estén interesados en la computación del aprendizaje de un perceptrón: adelante, sin miedo. Es un algoritmo relativamente simple, cosa que no va a ser el caso para redes multicapa, así que disfrutenlo mientras puedan.

### Parte 2.2. Algoritmo de aprendizaje

Sin más preámbulo, veamos cómo aprende el perceptrón.

Las entradas al perceptrón son fijas, son nuestros datos. Pero lo que podemos modificar son los pesos por los que las entradas se ven atravesadas. Sea $w_{ij}$ el peso de una entrada $i$. Se actualizará ese peso mediante la siguiente fórmula:

$$
w_{ij}(n+1) = w_{ij}(n) + \Delta w_{ij}(n)
$$

donde $\Delta w_{ij}(n)$ es lo que llamamos la Regla Delta, y tiene la siguiente forma:

$$
\Delta w_{kj}(n) = \eta e_{k}(n) X_{j}(n)
$$

Siendo $\eta$ el learning rate del perceptrón, otro hiperparámetro a configurar que "regula" que tan rapido aprende el perceptron, y $e_{k}(n)$ el término del error definido como:

$$
e_{k}(n) = t_{k}(n) - y_{k}(n)
$$

donde $t_{k}$ es la salida esperada, e $y_{k}$ es la salida real del perceptrón. Si son iguales, el error será nulo. De las ecuaciones se puede identificar que si no hay error, los pesos no cambian.

### Parte 2.3. Detalles algorítmicos

Con la introducción conceptual del perceptrón ya realizada, podemos entrar en algunos detalles del algoritmo. Al iniciar el entrenamiento, se suelen iniciar los pesos de forma aleatoria.

En el contexto de redes neuronales se habla de épocas, que son una pasada de todos los datos de entrada. Cuando se cumple la pasada de datos, se dice que pasó una época. El criterio de detención del algoritmo puede ser por cantidad de épocas, o bien observando que si pasó una época y los pesos no cambiaron, podemos considerar al aprendizaje terminado. 

## Parte 3. Conclusiones

El perceptrón simple es de alto interés conceptual, dado que nos permite dar los primeros pasos en el tema de redes neuronales, aunque rápidamente podamos ver sus limitaciones (datos linealmente separables). Quizas se esten preguntando como esquivamos la limitacion de que los problemas sean linealmente separables; la respuesta es _redes multicapa_, que sera el punto de interes de un articulo posterior. ¡No desesperen que ya llegará!

## Fuentes

Nada de este artículo inlcuye trabajo original. Parte del artículo fue escrito tomando información del libro *Artificial Intelligence, A Guide to Intelligent Systems* de Michael Negnevitsky, y mis notas de la cursada de Inteligencia Computacional, por lo que las notaciones de acá y las del libro pueden ser ligeramente diferentes.

## Links

Se adjunta un link con código para representar un perceptrón simple intentando aprender la función AND: [Google Colab Perceptron Simple](https://colab.research.google.com/drive/1-32C08gA2qTxSHv-iMVnBhf08YA_TAuG?usp=sharing)