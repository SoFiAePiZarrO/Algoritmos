# Resumen de hashing

El hashing es una técnica para almacenar y buscar datos de forma muy rápida. La idea central es convertir una clave en un índice de una tabla mediante una función hash.

## 1. Idea general

- Se tiene una tabla de tamaño $m$.
- Se aplica una función hash $h(k)$ a la clave $k$.
- El resultado indica en qué posición de la tabla se guarda o busca el elemento.

Ejemplo:

- Si la tabla tiene tamaño $10$ y la función es $h(k)=k\bmod 10$:
  - $h(23)=3$
  - $h(47)=7$
  - $h(12)=2$

## 2. ¿Qué es una colisión?

Una colisión ocurre cuando dos claves distintas producen el mismo índice.

Ejemplo:

- Con $m=10$:
  - $h(14)=4$
  - $h(24)=4$

Ambas claves quieren ir a la misma posición, así que hay que resolver la colisión.

## 3. Hashing con linear probing

En este método, si la posición calculada está ocupada, se recorre la tabla hacia adelante hasta encontrar una posición libre.

### Regla

- Se calcula $h(k)$.
- Si esa posición está ocupada, se prueba la siguiente.
- Si sigue ocupada, se sigue probando hasta encontrar un espacio libre.

### Ejemplo

Supongamos una tabla de tamaño $7$ y la función $h(k)=k\bmod 7$.

Insertamos:

1. $10 \to 3$
2. $17 \to 3$  → hay colisión, se prueba la posición $4$
3. $24 \to 3$  → hay colisión, se prueba la posición $5$

La tabla queda así:

| Índice | Valor |
|--------|-------|
| 0 | - |
| 1 | - |
| 2 | - |
| 3 | 10 |
| 4 | 17 |
| 5 | 24 |
| 6 | - |

### Ventajas

- Es simple de implementar.
- Usa solo una tabla.

### Desventajas

- Puede generar agrupamiento de datos, llamado clustering.
- En el peor caso, la búsqueda puede volverse lenta.

## 4. Hashing con encadenamiento

En este método, cada posición de la tabla contiene una lista (o cadena) de elementos que colisionan en ese índice.

### Regla

- Se calcula $h(k)$.
- Se agrega la clave a la lista correspondiente a esa posición.

### Ejemplo

Usamos la misma tabla de tamaño $7$ y la misma función $h(k)=k\bmod 7$.

Insertamos:

1. $10 \to 3$
2. $17 \to 3$  → va a la misma lista
3. $24 \to 3$  → también va a esa lista

La tabla queda así:

| Índice | Lista |
|--------|-------|
| 0 | - |
| 1 | - |
| 2 | - |
| 3 | [10, 17, 24] |
| 4 | - |
| 5 | - |
| 6 | - |

### Ventajas

- Es muy intuitivo.
- No depende de encontrar una posición vacía linealmente.
- Maneja bien las colisiones.

### Desventajas

- Requiere más memoria por los nodos o listas.
- En el peor caso, una lista puede ser muy larga.

## 5. Comparación rápida

| Método | Idea | Ventaja | Desventaja |
|--------|------|---------|------------|
| Linear probing | Busca la siguiente posición libre | Muy simple | Puede generar clustering |
| Encadenamiento | Guarda varias claves en una lista por índice | Maneja bien las colisiones | Usa más memoria |

## 6. Complejidad

En promedio, tanto el insertado como la búsqueda son cercanos a $O(1)$ si la tabla no está sobrecargada.

La carga de la tabla se mide con:

$$
\alpha = \frac{n}{m}
$$

donde:

- $n$ = número de elementos
- $m$ = tamaño de la tabla

Si la tabla se llena demasiado, el rendimiento empeora.

## 7. Regla práctica

- Un buen hashing necesita:
  - una función hash rápida,
  - una distribución uniforme de las claves,
  - una tabla con tamaño suficiente,
  - una estrategia clara para resolver colisiones.

En resumen, hashing permite acceder a datos casi en tiempo constante, y las colisiones se resuelven con técnicas como linear probing o encadenamiento.
