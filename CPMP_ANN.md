Training an ANN for solving CPMP
==
La idea es entrenar una red para resolver el CPMP.
Para hacerlo se entrena la red usando estados->acciones presentes en resoluciones óptimas del problema.



Pasos
--

**1. Generación de instancias**
Generar instancias 4 stacks x 4 contenedores (+2 espacios libres). Group value aleatorios entre 0 y 100.

**2. Generación de muestras**
Resolución óptima. Las muestras corresponden a todos los estados (x) con la acción correspondiente (y1,y2). Se pueden generar estados equivalentes haciendo intercambios entre stacks.

**3. Entrenamiento**
Usar red neuronal.
Se pueden entrenar dos redes: una que aprenda a seleccionar stacks de origen y otra que aprenda a seleccionar stacks de destino. 

La red de los stacks de destino debería recibir como entrada un estado modificado de tal manera que **siempre** la primera columna corresponda al stack de origen. Para esta red, la salida 0 no debería considerarse (stack destino = stack origen)

Para obtener buenos parámetros para la red se puede usar *RandomSearchCV*.

**4. Pruebas**
Intentar resolver instancias de prueba (100 instancias).  Reportar:
* Porcentaje de instancias resueltas
* Promedio de pasos en instancias resueltas vs. promedio óptimo en **mismas instancias**

Representación del estado
--
Los estados deberían ser representados por una matriz (o un vector ordenado por filas). En esta matriz los valores se encuentran:
* *compactados:* es decir, diferencias entre group_value seguidos son igual a 1.
* *normalizados:* dividir los group_value por max_group_value
* *elevados*: los top de los stacks en primera fila de la matriz, los espacios vacíos representados por 1.0
![cpmp_state_ann](https://docs.google.com/drawings/d/e/2PACX-1vQNLBGwH7vfOOtnZwdv0_26tHkpk_2FxjkDKQF_BeOBGL5e5Dgok7myEZwSoNizxTMmzm_o7W61cHnF/pub?w=960&h=723)
Creo que los tops de los stacks son generalmente los elementos más importantes. La elevación permite que los tops de cada stack se encuentren siempre en la primera fila de la matriz. De esta manera la red puede ubicarlos más fácilmente.



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3Mzc2NDIzMTEsMzAwOTM5NDEzLDU1MT
Q0NjAwNV19
-->