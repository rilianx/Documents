Interactive tree search for combinatorial  optimization problems
=

In this work we propose an *interactive tree search solver* for combinatorial optimization problem. On the one hand, it allows users to solve an optimization problem by exploring the search tree in their own way. On the other hand, the solver is able to learn *automatic* heuristics for selecting nodes imitating the decisions made by the user.

Nodes in the tree represents states in the resolution process, thus children of a node corresponds to different alternatives paths in the resolution. The user is allowed to select, in each iteration, the next node to *simulate and expand*. A simulation consists in automatically constructing a solution from the node following arbitrary (and maybe random) rules. A simulation gives us some idea about the quality that we can reach if we follow the node path. Some information is attached to the nodes (e.g., reach solution quality, mean, standard deviation) that can be used for the user to select the next node to simulate.

From the user behaviour, the solver learns a heuristic for selecting nodes automatically. The heuristic is based on a simple model and its parameters are adjusted in order to approach the decisions of the user.

> [Apuntes en Drive](https://docs.google.com/document/d/1CH2GFopOeL1EHA7i9j4Pn8T_HRM_2np9jmo_ACcgiuY/edit#heading=h.zcv4ojn5ljtv) ∙

Objetivos
-
 - Visualizador de árbol de búsqueda que evalúa nodos usando simulaciones de solver específico
 - Generar heurística que imite el comportamiento del usuario para seleccionar el mejor nodo

¿Cómo generar una heurística que imite el comportamiento del usuario? 
-
Primero que nada necesitamos:

**Una función heurística con parámetros** 
Básicamente como las que estamos generando en el solver. Es decir, una función que evalúe los nodos en base a sus distintos atributos (promedio, mejor, simulaciones, nodos en nivel, hijos, etc..)

Por ejemplo (similar a BeamSearch):
si el nodo ha sido seleccionado previamente:
$f_{A,B,C,D,E} = -A*n_{d+1} -B*depth - D*children + E*firstev$,
si no:
$f_{A,B,C,D,E} = -A*n_{d+1} - B*depth + -C*max(\sqrt{n_d}-sn_d,0) + E*firstev$,

donde:
$sn_d$: Cantidad de nodos marcados selected en el nivel $d$.
$n_d$: Cantidad de nodos en el nivel $d$
$firstev$: Es la primera evaluación del nodo

A, B, C, D y E son los **parámetros de la función**.
**Estos parámetros serán aprendidos por el modelo.**


**Calidad de la función**
La calidad se refiere a que tan bien imita la función $f(x)$ las decisiones tomadas por el usuario.

La calidad de la función se puede medir en base a un **conjunto de muestras.**

Estas muestras se pueden ir recolectando cuando el usuario interactúa con el sistema. Las muestras pueden consistir en una tupla:
lista de nodos para elegibles --> nodo escogido por el usuario 
Los **nodos elegibles** son los nodos hojas del árbol
![image](https://i.ibb.co/FJdt6NK/image.png)

Y el nodo escogido es el que se selecciona para simular.

La **calidad de una función** parametrizada $f_{A,B,C,D,E}$ se puede obtener observando en qué posición (normalizada entre 0 y 1) queda el nodo escogido por el usuario para cada una de las muestras de acuerdo a la función (ranking). 0 quiere decir que es el mejor nodo según la función y 1 que es el peor nodo según la función.

Entonces, por ejemplo, para un conjunto de 10 muestras, las posiciones podrían dar (0.1, 0.07, 0.2, 0.15, 0.0 ..) lo que indicaría que la función se acerca bastante a las decisiones tomadas por el usuario.


Implementación
---
**Fase de Extracción de datos**
La idea de la extracción es generar un archivo que almacene los estados del programa (información de los nodos seleccionables) y la decisiones hechas por el usuario (nodo seleccionado).
Más que una posición para cada nodo, lo que se debería almacener en el archivo es **toda la información** que necesita la heurística para evaluar nodos. Es decir, los datos de cada nodo seleccionable (mean, std, firstev, bestev, depth, #hijos, #nodos en nivel, etc...).

Siguiendo con tu idea, **justo cuando el usuario aprieta un nodo y antes de simularlo**:
1. Se recorren y guardan todos los nodos con sus atributos en un archivo
   * No es necesario ordenarlos 
   * No sólo los nodos hojas ya que eventualmente cualquiera podría ser seleccionado (error previo mío)

2. En el archivo también debemos indicar el nodo seleccionado por el usuario (puede ser el id solamente ya que sus datos deberían haber sido agregados previamente).
	* No es necesario evaluar ni nada. Esta es la fase de extracción en donde estamos recopilando las decisiones que va tomando el usuario.

**Evaluación de la heurística**
La heurística de evaluación de nodos es una función con parámetros que pueden ser ajustados.
Para calcular la calidad de la heurística debemos analizar la información recopilada en los archivos como lo explicas acá:
![image](https://i.imgur.com/cMQjjt3.png)


**Fase de entrenamiento**
Una vez que hemos recopilado suficiente información pasamos a la etapa de entrenamiento.

El objetivo del entrenamiento es maximizar la calidad de la función en base a las muestras. Para ello es necesario ajustar los valores de los parámetros.

Para encontrar los mejores parámetros se puede usar técnicas conocidas de optimización como **hill climbing** o **nelder-mead**.

A continuación se muestra un ejemplo de `hill climbing`
La función `eval_heuristic` 

````python
def hill-climbing(initial_vector):
   v = initial_vector
   best_quality = eval_heuristic(v)
   no_improvements = 0
   while no_improvements<50:
      i = random_parameter(v)
      old_value = v[i]
      v[i] = random_value (v[i]) #aumentar o disminuir hasta 10%
      quality = eval_heuristic(v)
      if quality > best_quality:
         best_quality = quality
         no_improvements = 0
      else:
         v[i]= old_value
         no_improvements += 1
````


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc1MzA2OTcwMCwxNzQzMzUwNjc4LDEzOD
MxODk2MzZdfQ==
-->