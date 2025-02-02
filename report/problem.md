# Problema

Dados $n$ trabajos $\{j_1, j_2, ..., j_n\}$, donde existe una dependencia acíclica entre ellos (para realizar algunos trabajos, es necesario haber realizado otros previamente). Cada trabajo $i$ consiste en $s$ operaciones, $\{O_{i,1}, O_{i,2}, ..., O_{i,s}\}$ que deben ser procesadas por $m$ computadoras en ese orden. Es posible detener la ejecución de una operación, con un costo de tiempo $C_{i,j}$ para la operación $j$ del trabajo $i$. Cada operación posee un porcentaje de progreso por segundo por computadora dado por $T_{i, j, k} \in \{1, 2, 3, ..., 100\}$ (porcentaje de progreso por segundo de la operación $j$ del trabajo $i$ en la computadora $k$). Nótese que cada computadora puede procesar a lo sumo una operación a la vez. El objetivo es **acabar todos los trabajos en el menor tiempo posible**.

**Nota**: Se asume que el tiempo es entero no negativo

## Entrada

- Un diccionario con *trabajo* como llave y como valor los *trabajos de los que depende*.
- Una lista en la que cada índice representa un *trabajo*, y sus elementos son listas que indican el *costo de interrupción de sus operaciones* (dispuestas en su orden de ejecución)
- Una lista en la cada índice representa un *trabajo*, y sus elementos constituyen matrices que indican el *porcentaje de progreso por segundo de una operación en una computadora*.


### Ejemplo 

**($3$ trabajos y $2$ computadoras)**

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

## Salida

Una lista en la que cada índice representa una *computadora* y sus elementos constituyen una tupla formada a su vez por dos tuplas: *intervalo de uso y operación realizada durante dicho intervalo*.

**Nota**: Se asume que toda salida debe ser factible

### Ejemplo

```python
[
    [((0, 5), (1, 1)), ((9, 12), (2, 1)), ((15, 20), (2, 2)), ((30, 35), (3, 1))],
    [((6, 9), (1, 2)), ((15, 17), (1, 3)), ((23, 28), (4, 1))],
]
```