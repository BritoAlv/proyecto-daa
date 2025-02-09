# Job Scheduling. Diseño y Análisis de Algoritmos

- Álbaro Suárez Valdés C-412
- Álvaro Luis González Brito C-412
- Javier Lima García C-412

## Definición del problema central (P0) (Job Scheduling)

Dados $n$ trabajos $J = \{j_1, j_2, ..., j_n\}$, un orden parcial $≺$ sobre $J$ (para realizar algunos trabajos, es necesario haber realizado otros previamente). Cada trabajo $i$ consiste en $s$ operaciones, $\{O_{i,1}, O_{i,2}, ..., O_{i,s}\}$ que deben ser procesadas por $m$ procesadores en ese orden. Es posible detener la ejecución de una operación, con un costo de tiempo $C_{i,j}$ para la operación $j$ del trabajo $i$. Cada operación posee un porcentaje de progreso por unidad de tiempo por procesador dado por $T_{i, j, k} \in \{1, 2, 3, ..., 100\}$ (porcentaje de progreso por unidad de tiempo de la operación $j$ del trabajo $i$ en el procesador $k$). Nótese que cada procesador puede procesar a lo sumo una operación a la vez. El objetivo es **acabar todos los trabajos en el menor tiempo posible**.

**Nota**: Se asume que el tiempo es entero no negativo

### Motivaciones

Una solución al problema dado puede tener mucha utilidad en la práctica; dado que numerosos procesos computacionales pueden ser modelados de esta manera. Por ejemplo: un proceso compuesto por varios programas (*trabajos*) relacionados por un orden, que a su vez están compuestos por una cantidad finita y secuencial de instrucciones (*operaciones*), cada uno de los cuales debe ser ejecutado en un conjunto de computadoras (procesadores) y además existe el costo de hacer un cambio de contexto (*costo de interrupción*); puede darse el caso, incluso, que algunas instrucciones son ejecutadas de manera más eficiente en algunas computadoras que en otras.

## Análisis de Complejidad

**Nota**: En esta sección trabajaremos con la versión de **"decisión"** de (P0), es decir, la versión en la que existe además un tiempo límite $t$ y la pregunta es: ¿es posible terminar todos los trabajos antes de $t$?.

Para analizar la complejidad temporal de (P0), enfoquémonos en los siguientes casos particulares.

### Definición (P1) (Tiempo de ejecución unitario)

Dada la definición de (P0), añadamos las siguientes restricciones:

- El porcentaje de progreso por unidad de tiempo es 100 para todas las operaciones por cada procesador ($T_{i, j, k} = 100, \space \forall i, j, k$). En otras palabras las operaciones tienen una *duración de una unidad de tiempo*, independientemente del procesador en la que sea ejecutada. De esta restricción podemos deducir que el costo de interrupción es despreciable.
- Todos los trabajos presentan solo una operación, $s_1 = s_2 = ... = s_n$.

### Definición (P2) (Tiempo de ejecución unitario con cantidad variable de procesadores)

Dada la definición de (P1), añadamos la restricción de que la cantidad de procesadores es variable con el tiempo, $c_0, c_1, ..., c_{t-1}$ tal que $\sum_{i = 0}^{t-1} c_i = n$, donde $c_i$ es la cantidad de procesadores en el instante $i$.

En lo que sigue asumimos que los problemas están codificados de tal forma que la longitud del string representativo de un problema de $n$ trabajos es polinomial en $n$ o al menos $O(n)$. Asumimos, además, que la codificación (encoding) es lo suficientemente natural para que los trabajos, la relación $≺$, etc, puedan ser determinados de manera sencilla y por supuesto en tiempo polinomial.

### Lema 1: (P2) se transforma polinomialmente en (P1)

#### Demostración:

Dada una instancia de (P2), introduzcamos nuevos trabajos $I_{ij}$ para $0 ≤ i < t$ y $0 ≤ j ≤ n - c_i$. Mantengamos a los antiguos trabajos relacionados por $≺$ y añadamos $I_{ij} ≺ I_{i-1, k}$ para $0 ≤ i < t - 1$ con $j$ y $k$ arbitrarios. Si seleccionamos $n + 1$ procesadores y un tiempo límite t, tenemos una instancia de (P1). 

##### Observación

*En cualquier solución, exactamente $n + 1 - c_i$ de los nuevos trabajos deben ser ejecutados en la iésima unidad de tiempo.*

Por motivos de contradicción, supongamos que las tareas $I_{kj}$ para un $k < t - 1$ y $0 ≤ j ≤ n - c_k$ son ejecutadas en la unidad de tiempo $k + 1$.

Sea $|{J_{k+1}, ..., J_{t-1}}| = p$, entonces tendremos que realizar un total de:

$\sum_{i = k + 1}^{t - 1} n + 1 - c_i = p (n + 1) + (c_{k+1}, ..., c_{t-1}) ≥ p(n + 1) - n = pn + p - n$

operaciones en:

$(p - 1)(n + 1) = pn + p - n - 1$

procesadores.

$pn + p - n - 1 < pn + p - n$, una **contradicción** (la cantidad de operaciones supera a la cantidad de procesadores)

∴ (P1) tiene una solución si y solo si la instancia original de (P2) tiene solución

Claramente la complejidad temporal de construir una instancia de (P1) es a los sumo cuadrática en la longitud de la representación de (P2). Luego, (P2) puede ser transformado polinomialmente en (P1)

■

### Lema 2: 3-SAT se reduce polinomialmente a (P2)

#### Demostración

Dada una instancia de 3-SAT con $m$ variables ($x_i, \space 1 ≤ i ≤ m$) y $n$ cláusulas ($Di, \space 1 ≤ i ≤ n$), construyamos la siguiente instancia de (P2).

Como trabajos:

- $x_{ij} \space$ y $\space \bar x_{ij}$ para $1 ≤ i ≤ m$ y $0 ≤ j ≤ m$
- $y_{i} \space$ y $\space \bar y_{i}$ para $1 ≤ i ≤ m$
- $D_{ij} \space$ para $\space 1 ≤ i ≤ n \space$ y $\space 1 ≤ j ≤ 7$

La relación $≺$ está dada por:

- $x_{ij} ≺ x_{i, j+1} \space$ y $\space \bar x_{ij} ≺ \bar x_{i, j+1}$, para $1 ≤ i ≤ m$ y $0 ≤ j ≤ m$

- $x_{i, i-1} ≺ y_{i} \space$ y $\space \bar x_{i, i-1} ≺ \bar y_{i}$, para $1 ≤ i ≤ m$

- Consideremos $D_{ij}$, donde $a_1a_2a_3$ constituye la representación binaria de $j$ (Notemos que el caso en el que $a_1 = a_2 = a_3 = 0$ no puede ocurrir). Además $D_i$ está formado por los literales $z_{k_1}, z_{k_2}, z_{k_3}$, donde cada $z$ representa de manera independiente a $x$ o $\bar x$, en un orden fijo. Entonces tenemos, para $1 ≤ p ≤ 3$, que $z_{k_p, m} ≺ D_{ij}$ si $a_p = 1$, o $\bar z_{k_p, m} ≺ D_{ij}$ si $a_p = 0$, donde $z$ representa a $\bar x$ or $x$, si $z$ representa a $x$ o $\bar x$, respectivamente.

El tiempo límite es $m + 3$, y las constantes $c_i, \space 0 ≤ i ≤ m + 2$ son:

- $c_0 = m$
- $c_1 = 2m + 1$
- $c_i = 2m + 2$, para $2 ≤ i ≤ m$
- $c_{m+1} = n + m + 1$
- $c_{m+2} = 6n$

Debemos demostrar que esta instancia de (P2) tiene solución si y solo si la instancia de 3-SAT tiene. La idea intuitiva detrás de la demostración está en que $x_i$ (o $\bar x_i$) es *true* si y solo si $x_{i0}$ (o $\bar x_{i0}$, respectivamente) es ejecutado en el instante 0. Notaremos que la presencia de los trabajos $y$'s y $\bar y$'s fuerzan a que exactamente uno de $x_{i0}$ y $\bar x_{i0}$ sea ejecutado en el instante 0 y el otro durante el instante 1. Luego, el requerimiento de que $n + m + 1$ trabajos sean ejecutados durante el instante $m + 1$ es equivalente al requerimiento de que por cada $i$, hay un $j$ tal que $D_{ij}$ puede ser ejecutado en ese instante (no puede haber más de uno). Pero esta condición es equiparable a decir que la cláusula $D_i$ tiene valor *true* cuando aquellos $x_i$'s y $\bar x_i$'s que fueron ejecutados en el instante 0 recibieron valor *true*.

Primero, demostremos que en cualquier solución de la instancia de (P2), no podemos ejecutar ambos $x_{i0}$ y $\bar x_{i0}$ en el instante 0 para cualquier $i$. Supongamos que sí. Entonces, dado que $c_0 = m$, existe un $j$ tal que ninguno de $x_{j0}$ y $\bar x_{j0}$ fue ejecutado en el instante 0. De ahí que ninguno de $y_j$ y $\bar y_j$ podrá ser ejecutado en o antes del instante j, debido a que $y_j$ debe ser precedido por $x_{j0}, x_{j1}, ..., x_{j, j-1}$, cada uno ejecutado de manera estricta a continuación del anterior. Luego el total de trabajos que pueden ser ejecutados en o antes del instante $j$ puede verse como:

- a lo sumo $m(2j + 1)$ de las $x$'s y $\bar x$'s, es decir, $z_{i0}, z_{i1}, ..., z_{ij}$ fueron ejecutados en el instante 0 y $z_{i0}, z_{i1}, ..., z_{i, j-1}$ si no (de nuevo z representa $x$ y $\bar x$)
- a lo sumo $2(j - 1)$ de los $y$'s, específicamente $y_1, \bar y_1, y_2, \bar y_2, ..., y_{j - 1}, \bar y_{j - 1}$

De ahí, tenemos, $2mj + 2j + m - 2$. Sin embargo, para $1 ≤ j ≤ m$,

$\sum_{i=0}^{j} c_i = 3m + 1 + (j - 1)(2m + 2) = 2mj + 2j + m - 1$ (**contradicción**)

Podemos, entonces, concluir, que en cualquier solución a esta instancia de (P2), exactamente uno de $x_{i0}$ y $\bar x_{i0}$ es ejecutado en el instante 0. Además, podemos determinar los trabajos exactos que son ejecutados en cada instante de tiempo entre 1 y m, dado cual de los dos, $x_{i0}$ o $\bar x_{i0}$ fue ejecutado en el instante 0. Dado que en el instante t tenemos que ejecutar $z_{it}$ si $z_{i0}$ fue ejecutado en el instante 0 y $z_{i, t-1}$ si no. Incluso, tenemos que ejecutar $y_t$ (respectivamente, $\bar y_t$) en el instante t si $x_{t0}$ (respectivamente, $\bar x_{t0}$) fue ejecutado en el instante 0 y ejecutar $y_{t - 1}$ (respectivamente, $\bar y_{t - 1}$) en el instante t si $x_{t0}$ (respectivamente, $\bar x_{t0}$) fue ejecutado en el instante 1.

En el instante $m + 1$ podemos ejecutar los m restantes $x$'s y $\bar x$'s y el restante $y$ o $\bar y$. Dado que $c_{m+1} = m + n + 1$, debemos ser capaces de ejecutar $n$ de los $D$'s si tenemos una solución. Observemos que por cada par $D_{ij}$ y  $D_{ij'}$, $j ≠ j'$, hay al menos un k tal que $x_km$ precede a $D_{ij}$ y $\bar x_km$ precede a $\bar D_{ij'}$, o viceversa. Como demostramos que exactamente uno de $x_km$ y $\bar x_km$ puede ser ejecutado en el instante m, se deduce que para cada i, a lo sumo uno de $D_{i1}, D_{i2}, ..., D_{i7}$ puede ser ejecutado en el instante $m + 1$.

Si asignamos el valor *true* a $x_k$, (respectivamente, $\bar x_k$) si y solo si $x_k0$ (respectivamente, $\bar x_k0$) fue ejecutado en el instante 0, entonces existirá uno de los $D_{i1}, D_{i2}, ..., D_{i7}$ ejecutable en el instante $m + 1$ si y solo si la cláusula toma valor *true* bajo esta asignación de las variables. 

Concluimos entonces que una solución a la instancia de (P2) existe si y solo si 3-SAT es satisfacible.

■

### Teorema 1: (P1) es NP-Completo

Por **Lema 1**, **Lema 2** y que 3SAT $\in$ NP-Completo 

■

### Corolario 1: (P0) es NP-Completo

Al ser (P1) un caso particular de (P0)

■

## Análisis de casos particulares

### Problema (P3) (Simple preemptive scheduling)

#### Definición

Dada la definición de (P0), añadamos las siguientes restricciones:

- $\forall i, j \space \exists p$ tal que $T_{i, j, k} = p, \space \forall k$. Es decir dada una operación su porcentaje de progreso por unidad de tiempo es el mismo para todos los procesadores
- Todos los trabajos presentan solo una operación, $s_1 = s_2 = ... = s_n$.
- El costo de interrupción es despreciable

#### Teorema 2: (P3) es NP-Completo

Notemos que (P1) es un caso particular de (P3) donde $p = 100$.

■

### Problema (P4) ()


## Solución propuesta para (P0)

### Entrada

- Un diccionario con *trabajo* como llave y como valor los *trabajos de los que depende*.
- Una lista en la que cada índice representa un *trabajo*, y sus elementos son listas que indican el *costo de interrupción de sus operaciones* (dispuestas en su orden de ejecución)
- Una lista en la cada índice representa un *trabajo*, y sus elementos constituyen matrices que indican el *porcentaje de progreso por segundo de una operación en un procesador*.


#### Ejemplo 

**($3$ trabajos y $2$ procesadores)**

```python
{
    3: [1, 2]
}
```

El trabajo $3$ depende de la culminación del $1$ y $2$ para poder comenzar.
 
```python 
[
    [20, 21, 22],
    [23],
    [24, 23]
]
```

El trabajo $1$ esta formado por $3$ operaciones, el $2$ por $1$ y el $3$ por $3$. Cada una presenta su respectivo costo de interrupción, por ejemplo: $C_{1, 2} = 21, C_{3, 1} = 24$ 
 
```python
[
    [
        [30, 35],
        [50, 55]
        [50, 40]
    ],
    [
        [30, 20]
    ],
    [
        [10, 12],
        [15, 20]
    ]
]
```

Porcentaje de progreso por segundo; por ejemplo: $T_{1, 1, 1} = 30, T_{1, 1, 2} = 35, T_{2, 1, 2} = 20, T_{3, 2, 1} = 15$

### Salida

Una lista en la que cada índice representa un *procesador* y sus elementos constituyen una tupla formada a su vez por dos tuplas: *intervalo de uso y operación realizada durante dicho intervalo*.

**Nota**: Se asume que toda salida debe ser factible

#### Ejemplo

```python
[
    [((0, 5), (1, 1)), ((9, 12), (2, 1)), ((15, 20), (2, 2)), ((30, 35), (3, 1))],
    [((6, 9), (1, 2)), ((15, 17), (1, 3)), ((23, 28), (4, 1))],
]
```