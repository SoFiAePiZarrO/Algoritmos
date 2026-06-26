# Explicación intuitiva de la partición de Quicksort

En este código no hay punteros como en C o C++; aquí `i`, `s` y `t` son índices de posiciones dentro del arreglo.

## Qué significa cada variable

- `i`: es la posición inicial del pivote. Al principio, el pivote está en `a[i]`.
- `k`: es la posición donde se eligió el pivote al azar. Se usa solo para sacar ese elemento desde su lugar original y ponerlo en la posición `i`.
- `s`: marca el límite entre la parte izquierda ya ordenada respecto al pivote y la parte derecha aún por revisar.
- `t`: recorre el arreglo para inspeccionar los elementos que aún no han sido clasificados.

La idea es esta:

La variable `k` no representa el pivote en sí, sino la posición desde la que se tomó el valor que luego se colocará en `i`. En otras palabras:

- `i` es la posición fija donde quedará el pivote durante la partición.
- `k` es una posición temporal usada para elegir ese pivote al azar.

Por eso, primero se hace esto:

```python
k = np.random.randint(i, j)
(a[i], a[k]) = (a[k], a[i])
```

Ese intercambio asegura que el elemento elegido al azar pase a quedar en `a[i]`, y desde ahí la partición puede trabajar como si el pivote estuviera en la posición `i`.

- a la izquierda del pivote van los elementos menores o iguales que él,
- a la derecha van los elementos mayores,
- y al final el pivote queda en el centro.

## Por qué aparecen `s` y `t`

Piensa en el arreglo como si estuviera dividido en tres zonas:

1. `a[i+1 .. s]`: ya sabemos que son menores o iguales que el pivote.
2. `a[s+1 .. t]`: son mayores que el pivote y todavía no se han movido.
3. `a[t+1 .. j]`: todavía no se han revisado.

El índice `t` va recorriendo los elementos que faltan por revisar. Cuando encuentra uno menor o igual que el pivote, lo mueve a la zona izquierda.

## Por qué se usa `t+1`

Porque `t` es el último índice que ya fue procesado. El siguiente elemento por mirar es precisamente `t+1`.

En otras palabras:

- `t` = “ya revisé hasta aquí”
- `t+1` = “ahora voy a revisar este elemento”

Eso es por qué en el `for` aparece:

```python
for t in range(s, j):
    if a[t+1] <= a[i]:
```

## Qué hace el ciclo

El ciclo recorre desde `s` hasta `j-1` y mira el elemento `a[t+1]`.

Si ese elemento es menor o igual que el pivote:

- lo intercambia con el siguiente lugar libre de la zona izquierda,
- y aumenta `s` en 1.

Así, `s` va creciendo cada vez que se encuentra un elemento que debe ir a la izquierda.

## Ejemplo paso a paso con la secuencia correcta

Usaremos exactamente la secuencia que diste:

```python
[5, 3, 8, 1, 7]
```

Y asumiremos que el pivote es `5`, que queda en la posición `i = 0`.

### Estado inicial

```python
[5, 3, 8, 1, 7]
```

- `i = 0`
- `s = 0`
- el pivote es `5`

### Primer elemento revisado

Ahora `t = 0`, así que miramos `a[1] = 3`.

Como `3 <= 5`, se hace este intercambio:

```python
(a[1], a[1])
```

En este caso no cambia nada porque el elemento ya está en la posición correcta para la zona izquierda del pivote.

El estado sigue siendo:

```python
[5, 3, 8, 1, 7]
```

Y ahora `s = 1`.

### Segundo elemento revisado

Ahora `t = 1`, así que miramos `a[2] = 8`.

Como `8 > 5`, no se hace intercambio.

Estado:

```python
[5, 3, 8, 1, 7]
```

### Tercer elemento revisado

Ahora `t = 2`, así que miramos `a[3] = 1`.

Como `1 <= 5`, se hace este intercambio:

```python
(a[2], a[3]) = (a[3], a[2])
```

Resultado:

```python
[5, 3, 1, 8, 7]
```

Y ahora `s = 2`.

### Cuarto elemento revisado

Ahora `t = 3`, así que miramos `a[4] = 7`.

Como `7 > 5`, no se hace intercambio.

### Final de la partición

Ahora el pivote se mueve a la posición `s`:

```python
(a[0], a[2]) = (a[2], a[0])
```

Resultado final:

```python
[1, 3, 5, 8, 7]
```

Aquí el pivote `5` ya quedó en el centro, con:

- elementos menores a la izquierda: `1, 3`
- elementos mayores a la derecha: `8, 7`

Ese es el efecto de la partición con la secuencia original `5, 3, 8, 1, 7`.

## Qué significa el invariante

La línea comentada:

```python
# invariante: a[i+1..s] <= a[i], a[s+1..t] > a[i]
```

quiere decir:

- los elementos desde `i+1` hasta `s` ya están bien ubicados a la izquierda del pivote,
- y los elementos desde `s+1` hasta `t` son mayores que el pivote.

## Resumen corto

- `i` marca la posición del pivote.
- `s` marca dónde termina la parte izquierda ya correcta.
- `t` recorre los elementos que aún falta clasificar.
- `t+1` es el siguiente elemento a revisar.

En resumen, la partición lo que hace es “separar” el arreglo en dos partes alrededor del pivote, sin perder ningún elemento.


## Cómo funciona Quicksort con mediana de 3

La versión con mediana de 3 es una mejora del Quicksort normal. La idea es la misma: dividir el arreglo en dos partes alrededor de un pivote, pero ahora el pivote se elige mejor.

En vez de tomar un pivote cualquiera, se miran tres elementos del subarreglo:

- el primero,
- el del medio,
- y el último.

Luego se escoge como pivote la mediana de esos tres valores. Eso ayuda porque el pivote suele quedar más cerca del centro del arreglo, y así las dos partes que se forman son más equilibradas.

### Idea general del algoritmo

1. Se toma el subarreglo entre `i` y `j`.
2. Se comparan `a[i]`, `a[(i+j)//2]` y `a[j]`.
3. Se elige la mediana de esos tres elementos como pivote.
4. Ese pivote se coloca en la posición inicial del subarreglo para trabajar con él como referencia.
5. Se reparte el arreglo de forma que:
   - a la izquierda queden los elementos menores o iguales al pivote,
   - a la derecha queden los mayores.
6. Se repite el proceso de forma recursiva en las dos mitades.

### Ejemplo paso a paso

Supongamos el arreglo:

```python
[5, 3, 8, 1, 7]
```

Para elegir el pivote por mediana de 3, miramos:

- `a[0] = 5`
- `a[2] = 8`
- `a[4] = 7`

La mediana de `5, 8, 7` es `7`.

Entonces el pivote será `7`.

Ahora el arreglo queda preparado para la partición, y la idea es mover los elementos menores que `7` a la izquierda y los mayores a la derecha.

#### Paso 1

Miramos `3`:

- `3 <= 7`, así que va a la izquierda.

Queda algo así:

```python
[3, 5, 8, 1, 7]
```

#### Paso 2

Miramos `8`:

- `8 > 7`, así que se queda a la derecha.

#### Paso 3

Miramos `1`:

- `1 <= 7`, así que también va a la izquierda.

Queda:

```python
[3, 1, 8, 5, 7]
```

#### Paso 4

Miramos `5`:

- `5 <= 7`, así que también va a la izquierda.

Al final, el pivote `7` se coloca en su posición correcta:

```python
[1, 3, 5, 7, 8]
```

### Por qué mejora al Quicksort normal

El Quicksort normal puede elegir un pivote muy malo, por ejemplo el menor o el mayor del subarreglo, y eso hace que una mitad quede muy pequeña y la otra muy grande.

Con mediana de 3, el pivote suele ser más cercano al centro, así que las dos particiones quedan más balanceadas y el algoritmo suele ser más rápido.

