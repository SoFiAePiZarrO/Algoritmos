# Resumen de BFS y DFS en grafos

## 1. Qué es un recorrido de grafos

Un recorrido de grafos consiste en visitar todos los vértices de un grafo siguiendo ciertas reglas. Dos de los recorridos más importantes son:

- BFS (Breadth-First Search): recorrido por niveles.
- DFS (Depth-First Search): recorrido en profundidad.

## 2. BFS (Breadth-First Search)

### Idea

El BFS visita primero los vértices más cercanos al inicio, es decir, recorre por niveles.

Se usa una cola como una especie de fila de espera: cuando se visita un vértice, sus vecinos se van agregando a la cola para ser visitados más tarde, en el orden en que fueron descubiertos.

### Cómo funciona

1. Se inicia en un vértice origen.
2. Se marca como visitado.
3. Se agregan sus vecinos a la cola.
4. Se saca un vértice de la cola y se repite el proceso con sus vecinos.

### Características

- Descubre primero los nodos más cercanos.
- Es útil para encontrar caminos más cortos en grafos no ponderados.
- Se implementa con una cola.

### Ejemplo

Supongamos este grafo:

```text
A - B - D
|   |
C - E
```

Si iniciamos en A, el recorrido BFS sería:

```text
A, B, C, D, E
```

### Explicación breve

- Desde A se visitan B y C.
- Luego desde B se visita D.
- Luego desde C se visita E.

## 3. DFS (Depth-First Search)

### Idea

El DFS va lo más profundo posible antes de volver atrás. Se explora un camino hasta el final y luego se prueba otro.

Se usa una pila para guardar los vértices por visitar.

### Cómo funciona

1. Se inicia en un vértice origen.
2. Se marca como visitado.
3. Se explora uno de sus vecinos.
4. Se sigue profundizando hasta que ya no hay más vecinos posibles.
5. Luego se vuelve atrás y se prueba otro camino.

### Características

- Va por caminos profundos primero.
- Es útil para encontrar componentes conexas, ciclos y recorridos de árboles.
- Se implementa con una pila o recursión.

### Ejemplo

Con el mismo grafo anterior, si iniciamos en A, un recorrido DFS podría ser:

```text
A, B, D, E, C
```

### Explicación breve

- Desde A se va a B.
- Desde B se va a D.
- Luego se vuelve y se explora E.
- Después se vuelve y se explora C.

## 4. Diferencia principal entre BFS y DFS

- BFS explora por niveles.
- DFS explora por profundidad.

### Resumen rápido

| Algoritmo | Estructura | Orden de exploración | Uso típico |
|----------|------------|----------------------|------------|
| BFS | Cola | Por niveles | Caminos más cortos |
| DFS | Pila/recursión | En profundidad | Exploración profunda |

## 5. Ejemplo comparativo

Consideremos este grafo:

```text
A -> B -> D
A -> C -> E
```

- BFS desde A: `A, B, C, D, E`
- DFS desde A: `A, B, D, C, E`

La diferencia es que BFS explora primero a los vecinos de A, mientras que DFS se va por un camino hasta el fondo antes de regresar.

## 6. Conclusión

BFS y DFS son dos formas básicas de recorrer grafos. La elección depende del problema:

- usar BFS cuando se quiere encontrar el camino más corto en un grafo no ponderado;
- usar DFS cuando se quiere explorar profundamente o buscar componentes, ciclos o rutas.
