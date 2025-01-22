## 1 - Distancia de árboles

Te dan dos enteros $x$ y $y$. Tu tarea es construir un árbol con las siguientes propiedades:

- El número de pares de vértices con una distancia par entre ellos es igual a $x$.
- El número de pares de vértices con una distancia impar entre ellos es igual a $y$.

Por un par de vértices, nos referimos a un par ordenado de dos (posiblemente, el mismo o diferente) vértices.

### Solución 

Para resolver este ejercicio vamos a apoyarnos en varias propiedades de los árboles:

Para cualquier árbol podemos tomar un nodo cualquiera como raíz y dividir los nodos de este en dos conjuntos, el conjunto A con los nodos que están a distancia par de la raíz y el conjunto B con los nodos que están a distancia impar de la raíz, la raíz se incluye en el conjunto A por estar a distancia 0.
Notemos que como $x$ son todos los pares de nodos a distancia par entre ellos y $y$ a distancia impar entonces $x + y$ son todos los pares de nodos del árbol, incluyendo los pares (u,u) y tomando como dos pares diferentes a (u,v) y (v,u) puesto que los pares son ordenados. Entonces siendo $n$ la cantidad de nodos que debe tener el árbol a construir se debe cumplir que:
$$x + y = 2* \binom{n}{2} + n$$
$$x + y = 2*n*(n-1)/2 + n$$
$$x + y = n^2$$
$$n = \sqrt{x + y}$$

Ahora demostremos otra propiedad, sea $w$ el nodo seleccionado como raíz y $u,v$ dos nodos cualesquiera del árbol:

- Si $u,v$ están a distancia par de $w$, como estamos en un árbol solo esiste un camino entre $u$ y $v$ y este representa su distancia, sea $d$ el nodo más alejado de $w$ que está en el camino de $w$ a $u$ y de $w$ a $v$, entonces el camino de $d$ a $u$ y de $d$ a $v$ es disjunto en vértices. Supongamos que $d$ está a distancia impar de $w$, entonces tanto $u$ como $v$ está a distancia impar de $d$, por lo que están a distancia par entre ellos, si $d$ está a distancia par de $w$ entonces $u$ y $v$ están a distancia par de $d$ y por ende están a distancia par entre ellos
- De forma análoga ocurre si $u$ y $v$ están a distancia impar de $w$, ellos están a dsitancia par de ellos
- Y siguiendo ese mismo razonamiento se ve que si $u$ está a distancia par de $w$ y $v$ a distancia impar de $w$ ellos están a distancia impar entre ellos y viceversa.

Sea $a = |A|$ y $n-a = |B|$ entonces se cumple que:

$$2* \binom{a}{2} + a + 2* \binom{n-a}{2} + n-a = x$$
$$2*a*(n-a) = y$$

Como la segunda fórmula es cuadrática aplicando la fórmula del discriminante podemos calcular el valor de $a$.
Y verificar si los valores obtenidos,conviertiendolos en enteros satisfacen todas las fórmulas, de lo contrario no tendría solución el problema
Teniendo el valor de a ya sabemos la cantidad de nodos que deben ir en cada conjunto.
Por lo que construir el árbol sería tomar un nodo como raíz, agreagerle $a-1$vértices adyacentes y a uno de esos vértices agregarle $n-a$ vértices adyacentes y tendríamos el árbol deseado
El código sería el siguiente:
```py
def tree(x,y):
    n = math.sqrt(x + y)
    a = Discriminante(n,y)
    if !es_valido(a):
        return false
    
    nodos = [0]
    aristas = []
    for i in range (1, a -1):
        nodos.add(i)
        aristas.add((0,i))
    
    for i in range (a, n-a):
        nodos.add(1)
        aristas.add((1,i))

    return nodos, aristas
```
Y este algoritmo en el caso peor es O(n) que en este caso sería O($\sqrt{x + y}$), notemos que no puede haber un algoritmo mejor, ya que el costo del algoritmo está dado por construir el árbol y para ello siempre hay que pasar al menos una vez por cada vértice a crear.

$$-*-$$

## 2 - Las cajas 

Dado que hay $n$ bolsas, cada una con $a_i$ pelotas amarillas y $b_i$ pelotas verdes, y cada caja puede contener $k$ pelotas, pero cada caja solo puede contener pelotas de la misma bolsa o pelotas del mismo color (amarillo o verdes):

Determina el número máximo de cajas que se puede llenar completamente.

### Solución

Sea $x_a,x_b$ el resto que deja un algoritmo determinado de pelotas amarillas y verdes respectivamente. Supongamos que tenemos inicialmente $\sum (a_i+b_i) = qk+c$ pelotas. Nótese que ocurre uno de los siguientes casos:

$$x_a+x_b=c$$

Este caso es óptimo y es la solución ideal tal que se llenan $q=\frac{\sum (a_i+b_i)}{k}$ cajas y por tanto es resto que queda es menor que $k$, o sea que con el resto no se puede llenar ninguna otra caja. Se utilizaron $qk$ pelotas. 

$$x_a+x_b=k + c$$

El algoritmo puede dejar como resto una cantidad $k+c$. No puede dejar más de $2k-2$ de resto porque ambos $x_a,x_b$ son menores que $k$, ya que, si uno de ellos fuese mayor que $k$ podríamos llenar una caja con ese color $\implies c < k$. Por tanto, el algoritmo que no llena $q$ cajas, llena necesariamente $q-1$ cajas, no menos.

Observemos también que en ambos casos tenemos el mismo $c$, porque a lo sumo se diferencian los casos en que el óptimo tiene una caja llena más que el segundo, y la cantidad total de pelotas en ambos es la misma.

Si $x_a$ es menor que $c \implies$ nos encontramos en el primer caso. Supongamos que estamos en el segundo caso con $x_a \le c \implies x_b \ge k$, por lo cual, al llenar una caja con $k$ pelotas verdes caeríamos en el primer caso. Por tanto, si $x_a$ es menor que $c \implies$ nos encontramos en el primer caso, de lo contrario nos encontramos en el segundo caso Luego, el problema se reduce a minimizar el resto de uno de los colores (sin pérdida de generalidad vamos a minimizar el resto de pelotas amarillas $x_a$).

Vamos a construir una matriz binaria $m$ de tamaño $n \times k$ donde por cada bolsa vamos a guardar todos los posibles resto de pelotas amarillas que pueden existir en esa bolsa. Ejemplo:

![](./images/2_01.jpg)

Con la primera bolsa se pueden formar los restos 0 y 1 porque nunca es posible dejar 2 pelotas amarillas con operaciones en la primera bolsa únicamente. Luego es fácil ver que en la segunda bolsa existen todos.

Para minimizar usaremos la siguiente dinámica en la cual tendremos la matriz binaria $dp$. Sea $dp[i][r]=1$ si las primeras $i$ bolsas pueden dejar de resto $r$ pelotas amarillas. Por lo cual, la respuesta sería la menor posición $j$ tal que $dp[n][j]=1$, o sea, el menor resto de pelotas amarillas con todas las bolsas. En la dinámica se van estableciendo los valores de la matriz $dp$ en la posición $i$ tomando los valores de la posición $i-1$ en la matriz $dp$ con los todos los valores de la posición $i$ en la matriz $m$ (hallando todas las posibles sumas y actualizando los valores de $dp[i]$).

La complejidad es $O(nk^2)$

$$-*-$$

## 3 - Subsecuencia X

Te dan un array de $N$ enteros como $A_1, A_2, A_3, \dots, A_N$. Tu tarea es encontrar el número de formas de dividir el array en subarrays contiguos tales que:

1. Cada elemento del array dado $A$ pertenezca exactamente a uno de los subarrays.
2. Existe un entero $m$, tal que el $X$ de cada subarray es igual a $m$.

El $X$ de una secuencia es el menor entero no negativo que no aparece en la secuencia.

### Solución

#### Solución en $O(N)$

`Observaciones del algoritmo`

- El $m$ de todas las subsecuencias debe ser el mismo e igual al del array $A$ completo. Supongamos que no $\implies$ existe una división en subsecuencias tal que su $m$ no es el $m$ del array completo. Sea $m_1$ el $m$ de las subsecuencias. Es fácil demostrar, asumiendo que una es menor que la otra, que ambas deben ser iguales.
- Si una secuencia tiene **MERX** igual a $m$, una secuencia mayor tendrá ese mismo **MERX**.
- $dp[i]$ : cantidad de formas de dividir el array $A[0:i]$ en subsecuencias tal que todas tienen el mismo $m$.
- $ac[i]$ : cantidad de formas de dividir todos los rangos $A[0:j]$ con $0 \le j \le i$ tal que cada una de las subsecuencias tienen el mismo $m$. Por definición $ac[0] = 1$
- $f[i]$ : posición $j\lt i$ más cercana a $i$ tal que el $m$ de $A[j:i]=m$

El array $f$ es llenado usando dos punteros, un arreglo de frecuencias de los valores en el rango $[0, m]$, y un contador para determinar cuándo se alcanza $m$.

`Implementación`

```py
for i in range(n):
    dp[i] = ac[f[i]-1]
    ac[i] = ac[i-1]+dp[i]
```

Para cada posición se analiza lo siguiente:
- $dp[i] = ac[f[i]-1]$ : la cantidad de formas de dividir el array $A[0:i]$ en subsecuencias con el mismo $m$. Analizas la última posición más cercana a $i$ tal que su **MERX** sea $m$, lo cual es $f[i]$, luego, a la cantidad de formas de dividir todos los rangos desde la posición anterior tal que cada una de las subsecuencias tienen el mismo $m$ se le añade un fragmento que contiene a $A[f[i]:i]$ para que complete hasta la posición $i$.
- $ac[i] = ac[i-1]+dp[i]$ : cantidad de formas de dividir todos los rangos hasta la posición $i$ se halla calculando la cantidad de formas de formas de dividir todos los rangos hasta la posición anterior y sumarle la cantidad de formas de dividir en subsecuencias hasta la posición $i$.

Nótese que no puede haber solución mejor porque lo mínimo que hay que hacer es recorrer el array, y dicha operación es $O(N)$.


## 4 - Subsecuencia Y

Dado un array binario $A$, encuentra la secuencia Y más larga $x_1, x_2, \dots, x_k$ tal que satisface:

$1 \leq x_1 < x_2 < \dots < x_k \leq N + 1$

$A[x_i : x_j - 1]$ contiene un número igual de ceros y unos para cada $i < j$.

### Solución

Notemos que para encontrar la subsecuencia más larga es necesario recorrer todo el array, por lo que la complejidad mínima es necesariamente mayor o igual que $O(n)$.

Un algoritmo con esta complejidad es el siguiente: digamos que $c[i]$ es igual a la cantidad de $1s$ menos la cantidad de $0s$ hasta la posicion $i$-1, esto podemos precalcularlo en $O(n)$.
 
Observemos que la solucion son las posiciones que más se repitan en $c$, pues si en $i,j$ se cumple que $c[i]=c[j]$, significa que entre dichas posiciones existe la misma cantidad de cada número. Y buscar dichas posiciones iguales es $O(n)$.

$$-*-$$

## 6 - Ganancias y pérdidas

Se tiene un grafo simple (es decir, un grafo sin lazos ni múltiples aristas) que consta de $n$ vértices y $m$ aristas.

El peso del $i$-ésimo vértice es $a_i$.

El peso de la $i$-ésima arista es $w_i$.

Un subgrafo de un grafo es un conjunto de vértices del grafo y un conjunto de aristas del grafo. El conjunto de aristas debe cumplir la condición de que ambos extremos de cada arista del conjunto deben pertenecer al conjunto de vértices elegido.

El peso de un subgrafo es la suma de los pesos de sus aristas, menos la suma de los pesos de sus vértices. Necesitas encontrar el peso máximo de un subgrafo del grafo dado. El grafo dado no contiene lazos ni múltiples aristas.

### Solución

Nótese que para llegar a una solución óptima es necesario recorrer todas las aristas y todos los nodos, por tanto, la complejidad mínima debe ser $O(V+E)$. A continuación describiremos un algoritmo con esa complejidad.

El subgrafo óptimo es evidente que no debe tener aristas negativas, por lo que, se realiza una primera pasada eliminando estas aristas. Calculamos en un array de tamaño $|V|$ la diferencia entre las aristas que le corresponden a cada nodo y su peso, o sea, siendo $v$ el nodo y $E(v)$ sus aristas, se calcula:

$$(\sum_{e \ \in \ E(v)}e) \ - v \ \ \forall \ v  \in V$$

Busquemos ahora recorriendo el array creado cuales nodos nos afectan el peso del grafo, lo que significa que buscaremos en el array las posiciones cuyo valor es negativo o cero y eliminamos esos nodos (junto a sus aristas), lo cual, mejorará el peso del grafo siempre, lo que implica que siempre estaremos pasando de un estado del grafo a uno mejor en cuanto a peso.

Puede pasar que al eliminar un nodo con sus aristas se afecte el cálculo de otros nodos ya visitados al punto de cambiar el signo de estos, por lo que sería necesario volver a visitarlos pero eso comprometería la complejidad. Esto se resulve creando una cola y al eliminar un nodo, añadir los nodos afectados a esa cola para visitarlos inmediatamente, eso implica que el costo del recorrido sería $O(V+E)$ y por tanto, la complejidad total del algoritmo sería la misma.



$$-*-$$

## 9 - Conectar todo

Dado un conjunto de $N$ nodos, donde cada nodo tiene un valor asociado $A_i$, se deben conectar todos los nodos mediante un conjunto de aristas bidireccionales. Una arista de longitud $L$ $(L > 0)$ puede ser construida entre los nodos $X$ e $Y$ si $(A_X \& A_Y \& L) = L$, donde $ \& $ representa el operador AND a nivel de bits.

Se solicita determinar la longitud mínima total de las aristas necesarias para conectar todos los nodos, o imprimir $-1$ si no es posible conectarlos.

- Dos nodos $X$ e $Y$ se consideran conectados si existe una secuencia de nodos $C_1, C_2, \dots, C_K$ $(K \geq 1)$ tal que $C_1 = X$, $C_K = Y$, y existe una arista entre $C_i$ y $C_{i+1}$ $(1 \leq i < K)$.
- Todos los nodos están conectados si cualquier par de nodos está conectado directa o indirectamente.

### Solución

Sea $k$ la mínima cantidad de *bits* para representar cada número de los nodos, y $A$ un array de tamaño $k$, tal que en la posición $i$ se guardarán todos los nodos cuyo *bit* menos significativo sea el $i$-$esimo$. Hasta ahora tenemos una complejidad de $O(n+k)$.

Recorremos ese array aplicando las siguientes operaciones:
- Si hay un solo nodo llevarlo a la posición del próximo *bit* menos significativo, y si hay más de un nodo aplicar la siguiente operación
- Si hay más de un nodo con ese *bit* encendido conectarlos con costo $(m-1)\times 2^i$ siendo $m$ la cantidad de nodos en esa posición. Hacer **OR** a todos esos nodos y al resultado aplicarle la operación anterior.
- Si no hay nodos continuar a la siguiente posición

Al finalizar, si resulta en un solo nodo en el array significa que todos los nodos han sido conectados en $O(n + k)$, de lo contrario, no es posible conectarlos todos y por tanto el algoritmo retorna -1.

$$-*-$$

## 11 - 3 puntos

Proporcione un algoritmo para determinar si en un plano hay 3 puntos alineados. Proporcionar un argumento por el cual se supone que no existe una solución con complejidad temporal polinomialmente menor


### Solución

#### Algoritmo en $n^2$

Creamos 3 arrays $X$, $Y$ y $Y'$. Guardamos los $n$ puntos ordenados por las $x$ en $X$ y por las $y$ en $Y$. Recorremos el array $X$ y por cada punto $c$ aplicamos el siguiente análisis (teniendo en cuenta que en $Y'$ iremos agregando los puntos de $X$ una vez visitados ordenados por las $y$):
- Extraemos $c$ del conjunto de $Y$
- Ponemos un puntero $p_1$ en $Y'$ desde el inicio y un puntero $p_2$ en $Y$ desde el final, y vamos buscando por los puntos de ambos arrays cuales forman vectores con $c$ que tengan la misma pendiente. EL análisis sería el siguiente:

![](./images/11_01.jpg)

![](./images/11_02.jpg)

![](./images/11_03.jpg)

Nótese que en cada vez que se hace una insersión en $Y'$ es $O(N)$, porque se están agregando elementos ya ordenados, al igual que cuando se busca con ambos punteros por los arrays $Y$ y $Y'$, y como esto se hace por cada elemento de $X$ la complejidad resulta en $O(N^2)$

## 
