# NP-Completeness

En lo que sigue asumimos que los problemas están codificados de tal forma que la longitud del string representativo de un problema de $n$ trabajos es polinomial en $n$ o al menos $O(n)$. Asumimos, además, que la codificación (encoding) es lo suficientemente natural para que los trabajos, la relación $≺$, etc, puedan ser determinados de manera sencilla y por supuesto en tiempo polinomial.

## Lema 1: (P4) se transforma polinomialmente en (P2)

### Demostración:

Dada una instancia de (P4), introduzcamos nuevos trabajos $I_{ij}$ para $0 ≤ i < t$ y $0 ≤ j ≤ n - c_i$. Mantengamos a los antiguos trabajos relacionados por $≺$ y añadamos $I_{ij} ≺ I_{i-1, k}$ para $0 ≤ i < t - 1$ con $j$ y $k$ arbitrarios. Si seleccionamos $n + 1$ procesadores y un tiempo límite t, tenemos una instancia de (P2). 

#### Observación

*En cualquier solución, exactamente $n + 1 - c_i$ de los nuevos trabajos deben ser ejecutados en la iésima unidad de tiempo.*

Por motivos de contradicción, supongamos que las tareas $I_{kj}$ para un $k < t - 1$ y $0 ≤ j ≤ n - c_k$ son ejecutadas en la unidad de tiempo $k + 1$.

Sea $|{J_{k+1}, ..., J_{t-1}}| = p$, entonces tendremos que realizar un total de:

$\sum_{i = k + 1}^{t - 1} n + 1 - c_i = p (n + 1) + (c_{k+1}, ..., c_{t-1}) ≥ p(n + 1) - n = pn + p - n$

operaciones en:

$(p - 1)(n + 1) = pn + p - n - 1$

procesadores.

$pn + p - n - 1 < pn + p - n$, una **contradicción** (la cantidad de operaciones supera a la cantidad de procesadores)

∴ (P2) tiene una solución si y solo si la instancia original de (P4) tiene solución

Claramente la complejidad temporal de construir una instancia de (P2) es a los sumo cuadrática en la longitud de la representación de (P4). Luego, (P4) puede ser transformado polinomialmente en (P2)

■

## Lema 2: 3-SAT se reduce polinomialmente a (P4)

### Demostración

Dada una instancia de 3-SAT con $m$ variables ($x_i, \space 1 ≤ i ≤ m$) y $n$ cláusulas ($Di, \space 1 ≤ i ≤ n$), construyamos la siguiente instancia de (P4).

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

Debemos demostrar que esta instancia de (P4) tiene solución si y solo si la instancia de 3-SAT tiene. La idea intuitiva detrás de la demostración está en que $x_i$ (o $\bar x_i$) es verdadero si y solo si $x_{i0}$ (o $\bar x_{i0}$, respectivamente) es ejecutado en el instante 0. Notaremos que la presencia de los trabajos $y$'s y $\bar y$'s fuerzan a que exactamente uno de $x_{i0}$ y $\bar x_{i0}$ sea ejecutado en el instante 0 y el otro durante el instante 1. Luego, el requerimiento de que $n + m + 1$ trabajos sean ejecutados durante el instante $m + 1$ es equivalente al requerimiento de que por cada $i$, hay un $j$ tal que $D_{ij}$ puede ser ejecutado en ese instante (no puede haber más de uno). Pero esta condición es equiparable a decir que la cláusula $D_i$ tiene valor verdadero cuando aquellos $x_i$'s y $\bar x_i$'s que fueron ejecutados en el instante 0 recibieron valor verdadero.

Primero, demostremos que en cualquier solución de la instancia de (P4), no podemos ejecutar ambos $x_{i0}$ y $\bar x_{i0}$ en el instante 0 para cualquier $i$. Supongamos que sí. Entonces, dado que $c_0 = m$, existe un $j$ tal que ninguno de $x_{j0}$ y $\bar x_{j0}$ fue ejecutado en el instante 0. De ahí que ninguno de $y_j$ y $\bar y_j$ podrá ser ejecutado en o antes del instante j, debido a que $y_j$ debe ser precedido por $x_{j0}, x_{j1}, ..., x_{j, j-1}$, cada uno ejecutado de manera estricta a continuación del anterior. Luego el total de trabajos que pueden ser ejecutados en o antes del instante $j$ puede verse como:

- a lo sumo $m(2j + 1)$ de las $x$'s y $\bar x$'s, es decir, $z_{i0}, z_{i1}, ..., z_{ij}$ fueron ejecutados en el instante 0 y $z_{i0}, z_{i1}, ..., z_{i, j-1}$ si no (de nuevo z representa $x$ y $\bar x$)
- a lo sumo $2(j - 1)$ de los $y$'s, específicamente $y_1, \bar y_1, y_2, \bar y_2, ..., y_{j - 1}, \bar y_{j - 1}$

De ahí, tenemos, $2mj + 2j + m - 2$. Sin embargo, para $1 ≤ j ≤ m$,

$\sum_{i=0}^{j} c_i = 3m + 1 + (j - 1)(2m + 2) = 2mj + 2j + m - 1$ (**contradicción**)

Podemos, entonces, concluir, que en cualquier solución a esta instancia de (P4), exactamente uno de $x_{i0}$ y $\bar x_{i0}$ es ejecutado en el instante 0. Además, podemos determinar los trabajos exactos que son ejecutados en cada instante de tiempo entre 1 y m, dado cual de los dos, $x_{i0}$ o $\bar x_{i0}$ fue ejecutado en el instante 0. Dado que en el instante t tenemos que ejecutar $z_{it}$ si $z_{i0}$ fue ejecutado en el instante 0 y $z_{i, t-1}$ si no. Incluso, tenemos que ejecutar $y_t$ (respectivamente, $\bar y_t$) en el instante t si $x_{t0}$ (respectivamente, $\bar x_{t0}$) fue ejecutado en el instante 0 y ejecutar $y_{t - 1}$ (respectivamente, $\bar y_{t - 1}$) en el instante t si $x_{t0}$ (respectivamente, $\bar x_{t0}$) fue ejecutado en el instante 1.

En el instante $m + 1$ podemos ejecutar los m restantes $x$'s y $\bar x$'s y el restante $y$ o $\bar y$. Dado que $c_{m+1} = m + n + 1$, debemos ser capaces de ejecutar $n$ de los $D$'s si tenemos una solución. Observemos que por cada par $D_{ij}$ y  $D_{ij'}$, $j ≠ j'$, hay al menos un k tal que $x_km$ precede a $D_{ij}$ y $\bar x_km$ precede a $\bar D_{ij'}$, o viceversa. Como demostramos que exactamente uno de $x_km$ y $\bar x_km$ puede ser ejecutado en el instante m, se deduce que para cada i, a lo sumo uno de $D_{i1}, D_{i2}, ..., D_{i7}$ puede ser ejecutado en el instante m + 1


