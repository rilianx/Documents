---


---

<h1 id="distancia-más-corta-entre-a-y-b">Distancia más corta entre A y B</h1>
<p>Para encontrar el camino más corto entre dos ciudades (A,B) puede usar un <strong>algoritmo de búsqueda</strong>  que comience a explorar el mapa de ciudades a partir de la ciudad A.</p>
<p>Cada <strong>estado</strong> o nodo del grafo representará la ciudad en la que se encuentra actualmente junto a la distancia recorrida para llegar a ella.<br>
Por lo tanto una <strong>acción</strong> o arco representa ir de una ciudad a otra durante el recorrido.</p>
<p>Se sugiere que use la siguiente estructura para el estado/nodo</p>
<pre class=" language-c"><code class="prism  language-c"><span class="token keyword">typedef</span> <span class="token keyword">struct</span><span class="token punctuation">{</span>
   <span class="token keyword">char</span> ciudad<span class="token punctuation">[</span><span class="token number">30</span><span class="token punctuation">]</span><span class="token punctuation">;</span>
   <span class="token keyword">int</span> dist<span class="token punctuation">;</span> <span class="token comment">/* distancia desde ciudad A */</span>
   Node<span class="token operator">*</span> prev<span class="token punctuation">;</span> <span class="token comment">/*nodo anterior para poder 
             reconstruir el camino*/</span>
<span class="token punctuation">}</span>Node<span class="token punctuation">;</span>
</code></pre>
<p>Luego, la función para obtener nodos adyacentes <code>get_adyacent_nodes(Node* actual)</code> debería generar los nodos posibles a partir del nodo <code>actual</code>.</p>
<p>Es decir, debe:</p>
<ol>
<li>revisar las ciudades conectadas a la ciudad actual</li>
<li>crear un nodo para cada una de ellas, inicializando la distancia como: <code>actual-&gt;dist + distancia_entre_actual_y_adyacente</code></li>
</ol>
<p>Una vez armado el grafo implícito puede implementar una búsqueda (en profundidad o anchura).</p>
<p>Algunas consideraciones al implementar el algoritmo de búsqueda:</p>
<ul>
<li>Guarde en una variable  la menor distancia encontrada hasta el momento (<code>min_dist=10000</code>) y el mejor nodo final (<code>best_node=NULL</code>).</li>
<li>Un nodo es <strong>final</strong> si corresponde a la ciudad de destino (B)</li>
<li>Cada vez que saque un nodo final, revise si corresponde a un camino más corto que el mejor encontrado (<code>min_dist</code>).  Si es así, actualice <code>min_dist</code> y <code>best_node</code></li>
<li>Descarte los nodos con distancia mayor a <code>mindist</code> (no agregue sus adyacentes a la pila/cola).</li>
<li>Use una <strong>búsqueda en anchura</strong> (la búsqueda en profundidad se puede quedar <em>atorada</em> en un bucle infinito).</li>
</ul>
<p>La distancia más corta será <code>min_dist</code> y para recuperar el camino óptimo debe comenzar con <code>aux=best_node</code> e iterar <code>aux=aux-&gt;prev</code> para obtener todos los nodos.</p>
<h2 id="mejora-algoritmo-de-dijkstra">Mejora (algoritmo de Dijkstra)</h2>
<p>Si bien el algoritmo mencionado encuentra el camino óptimo, podemos hacer que funcione un poco más rápido.</p>
<p>Para ello puede usar una <em>cola con prioridad</em> para almacenar los nodos (en vez de una cola). Si usa como <em>prioridad</em> la distancia del nodo y selecciona en cada iteración el nodo <em>más cercano a A</em>, estará implementando el <strong>algoritmo de Dijkstra</strong>.</p>
<p>La gracia de esta variante es que explora primero las regiones <em>más prometedoras</em> del grafo por lo que llegará a mejores soluciones en menos iteraciones.</p>

