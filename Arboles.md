# Algoritmos

## Recetas rápidas para resolver ejercicios de árboles del Control 2

Cuando aparezca un problema sobre árboles, conviene seguir este esquema general:

1. Identificar qué operación pide: búsqueda, inserción, eliminación, validación, recorrido o balanceo.
2. Recordar la propiedad del tipo de árbol involucrado.
3. Pensar en qué sucede en el nodo actual y luego en cuál hijo corresponde seguir.
4. Si la estructura cambia, propagar el cambio hacia arriba o reequilibrar.

### 1) ABB (árboles de búsqueda binaria)

Patrón de solución:
- Cada nodo cumple la propiedad: todo lo de la izquierda es menor y todo lo de la derecha es mayor.
- Para buscar, insertar o eliminar, se compara la clave con el nodo actual y se sigue por la rama correspondiente.
- Para verificar si un árbol es ABB, conviene usar rangos o recorrerlo en in-order.

Receta:
- Buscar: si la clave coincide, se encontró; si es menor, ir a la izquierda; si es mayor, ir a la derecha.
- Insertar: seguir el camino hasta una hoja y agregar allí el nuevo nodo.
- Eliminar: si no tiene hijos, quitarlo; si tiene un hijo, reemplazarlo por ese hijo; si tiene dos hijos, reemplazar por el sucesor o predecesor en in-order.

### 2) AVL

Patrón de solución:
- Es un ABB con la condición extra de que el balance de cada nodo debe estar en $-1$, $0$ o $1$.
- Después de insertar o eliminar, se sube por el camino hacia la raíz y se corrige el desbalance con rotaciones.

Receta:
- Calcular la altura de cada subárbol.
- Determinar el factor de balance del nodo.
- Si el valor es mayor que $1$ o menor que $-1$, aplicar la rotación correcta: LL, LR, RR o RL.

#### Rotaciones

En un AVL, cuando un nodo queda desbalanceado después de insertar o eliminar, se corrige con rotaciones. El desbalance se detecta con el factor de balance:

- factor de balance = altura(izq) - altura(der)
- si es mayor que 1 o menor que -1, hay que reequilibrar.

##### Rotaciones simples

1. Rotación simple a la derecha (LL)
   - Se aplica cuando el subárbol izquierdo del hijo izquierdo del nodo está más alto.
   - El hijo izquierdo sube y reemplaza al nodo padre.


2. Rotación simple a la izquierda (RR)
   - Se aplica cuando el subárbol derecho del hijo derecho del nodo está más alto.
   - El hijo derecho sube y reemplaza al nodo padre.


##### Rotaciones dobles

3. Rotación doble izquierda-derecha (LR)
   - Se aplica cuando el hijo izquierdo del nodo tiene un subárbol derecho más alto.
   - Primero se rota a la izquierda el hijo izquierdo, y luego a la derecha el nodo padre.

4. Rotación doble derecha-izquierda (RL)
   - Se aplica cuando el hijo derecho del nodo tiene un subárbol izquierdo más alto.
   - Primero se rota a la derecha el hijo derecho, y luego a la izquierda el nodo padre.

##### Forma de recordarlas

- LL → rotación simple a la derecha
- RR → rotación simple a la izquierda
- LR → rotación doble izquierda-derecha
- RL → rotación doble derecha-izquierda

En términos visuales, también se suelen llamar:

- zig-zig: caso LL o RR
- zig-zag: caso LR o RL

La idea general es siempre la misma: subir el nodo que está “demasiado profundo” y reencadenar los subárboles completos sin perder ninguna rama.

### 3) Árboles 2-3

Patrón de solución:
- Cada nodo puede tener una o dos claves y dos o tres hijos.
- La búsqueda compara con la primera y, si corresponde, con la segunda clave.
- La inserción puede provocar que un nodo se llene y luego se divida, promoviendo la clave del medio hacia arriba.

Receta:
- Buscar: si la clave está en el nodo, se encontró; si es menor que la primera clave, ir al hijo izquierdo; si está entre la primera y la segunda, ir al hijo intermedio; si es mayor que la segunda, ir al hijo derecho.
- Insertar: agregar en la hoja correspondiente; si el nodo se llena, dividirlo y subir la clave central.

### 4) B-trees

Patrón de solución:
- Son árboles de orden $m$ con hasta $m-1$ claves y $m$ hijos.
- La búsqueda se realiza con una búsqueda interna dentro del nodo y luego un descenso.
- La inserción y eliminación suelen implicar dividir, redistribuir o fusionar nodos.

Receta:
- Mantener las claves ordenadas dentro de cada nodo.
- Si un nodo tiene demasiadas claves, separarlo por la mitad y subir la clave del medio.
- Si un nodo queda con pocas claves, tratar de redistribuir o fusionar con un hermano.

### 5) Árboles digitales o tries

Patrón de solución:
- No se comparan valores completos, sino bits o caracteres uno por uno.
- Cada nivel del árbol representa un símbolo o un bit del dato.
- El nodo final de una palabra suele marcarse con una bandera especial.

Receta:
- Recorrer carácter por carácter o bit por bit.
- Si el siguiente camino no existe, crearlo.
- Al terminar la palabra, marcar el nodo como terminal.

### 6) Cómo decidir rápidamente qué enfoque usar

- Si el problema habla de “menor que” y “mayor que”, probablemente es ABB o AVL.
- Si menciona “balanceo automático”, “rotaciones” o “factor de balance”, es AVL.
- Si aparecen “dos claves por nodo” o “tres hijos”, es un árbol 2-3.
- Si aparece “orden del árbol”, “mínimo grado” o “muchos hijos”, es un B-tree.
- Si el problema es sobre palabras, prefijos, bits o códigos, es un árbol digital o trie.

### Regla de oro

Antes de programar, escribe la propiedad del árbol en una frase corta y luego resuelve el problema como si fueras a responder tres preguntas:
- ¿Qué debo verificar en este nodo?
- ¿A qué hijo me voy?
- ¿Qué cambio debo propagar hacia arriba?

### Cómo pensar las reestructuraciones de un árbol

Una fuente muy común de errores es confundir tres cosas distintas:

1. El puntero al nodo, por ejemplo `p`.
2. La información del nodo, por ejemplo `p.info`.
3. El subárbol completo que cuelga de ese nodo.

En una implementación típica, un nodo no es solo un valor. Un nodo representa este conjunto:

- `p.info`: el valor o clave que guarda el nodo.
- `p.izq`: el árbol izquierdo, es decir, el subárbol completo que queda a la izquierda.
- `p.der`: el árbol derecho, es decir, el subárbol completo que queda a la derecha.

Por eso, cuando uno dice “el árbol”, en realidad se refiere a todo lo que empieza en un nodo y contiene todos sus descendientes. Un nodo con sus hijos es un árbol. Un nodo vacío es también un árbol especial, que suele llamarse `Nodoe` o `None`.

#### Idea clave

Si quieres que el hijo de un nodo ocupe su lugar, no debes reemplazar solo la `info`.
Debes reemplazar el vínculo completo del árbol.

Es decir:

- incorrecto: `p.info = p.izq.info`
- incorrecto: `p = p.izq.info`
- correcto: `p = p.izq`

#### Ejemplo 1: eliminar un nodo con un solo hijo izquierdo

Supongamos que `p` es el nodo a eliminar y que tiene hijo izquierdo.

Lo correcto es:

- hacer que el padre de `p` apunte al subárbol completo de `p.izq`
- no solo copiar el valor del hijo

En notación simple:

```python
if p.izq is not None and p.der is None:
    padre.hijo = p.izq
```

¿Por qué funciona?
Porque `p.izq` no es solo un valor: es el árbol completo que empieza en ese hijo. Ese árbol conserva todos sus propios hijos, nietos, etc.

#### Ejemplo 2: eliminar un nodo con un solo hijo derecho

```python
if p.der is not None and p.izq is None:
    padre.hijo = p.der
```

Aquí también se reemplaza el nodo por todo el subárbol derecho, no solo por su información.

#### Ejemplo 3: reemplazar el valor, no la estructura

A veces lo que se quiere no es mover todo el subárbol, sino cambiar el valor del nodo actual.
Por ejemplo:

```python
p.info = hijo.info
```

Esto solo modifica el contenido del nodo `p`. No reconstruye la estructura ni mueve los hijos. Si el nodo `p` tenía hijos, siguen ahí. Si el hijo tenía también sus propios hijos, esos siguen intactos si se hizo la sustitución correcta del valor.

#### Ejemplo 4: cuando el nodo tiene dos hijos

En este caso, muchas veces se usa el sucesor o predecesor en in-order.
La idea es:

1. encontrar el nodo que reemplazará a `p`
2. copiar su `info` a `p.info`
3. eliminar ese nodo reemplazante desde su posición original

Es decir, en este caso se cambia el valor del nodo `p`, pero no se “mueve” el subárbol completo como en el caso de un solo hijo.

#### Ejemplo 5: rotación a la derecha y a la izquierda

En AVL, una rotación sirve para subir un hijo y bajar al padre cuando el árbol queda desbalanceado.

Rotación a la derecha:

```text
Antes                     Después
      8                         5
     / \                       / \
    5   10                    3   8
   / \                           / \
  3   6                         6   10
```

La idea es: el hijo izquierdo de la raíz sube y reemplaza a la raíz.

Ahora, la clave importante es esta:

- `pivot` no es solo el valor `5`.
- `pivot` es el nodo completo que representa al subárbol que empieza en `5`.
- Por eso, cuando haces `pivot.der = parent`, lo que estás haciendo es conectar el nodo `parent` como hijo derecho de ese subárbol completo.
- Y cuando haces `parent.izq = pivot.der`, lo que estás moviendo no es solo el `6`, sino el subárbol completo que estaba a la derecha de `pivot`.

En otras palabras:

- `pivot.der` no significa “el 6” como valor aislado.
- `pivot.der` significa “el subárbol derecho del nodo `5`”, que puede contener a `6` y sus hijos.

Si el nodo `5` tuviera hijos, esos hijos se conservan; lo que se reacomoda es la conexión entre nodos, no solo la información.


```python
def rotate_right(self, parent):
    pivot = parent.izq
    parent.izq = pivot.der
    pivot.der = parent
    return pivot
```


#### Qué cambia exactamente en `rotate_right`
Cada parte significa esto:

- `parent` = el nodo que está arriba y que se va a bajar.
- `pivot` = el hijo izquierdo de `parent`, el que sube.
- `parent.izq = pivot.der` = el subárbol derecho de `pivot` pasa a ser el hijo izquierdo de `parent`.
- `pivot.der = parent` = el nodo `parent` pasa a ser el hijo derecho de `pivot`.
- `return pivot` = la nueva raíz de esa parte del árbol es `pivot`.

Es decir, el nodo `pivot` ocupa el lugar de `parent`, y `parent` baja un nivel.

Rotación a la izquierda:

```text
Antes                     Después
      5                         8
     / \                       / \
    3   8                     5   10
       / \                   / \
      6   10                3   6
```


La idea es: el hijo derecho de la raíz sube y reemplaza a la raíz.

Y aquí ocurre lo mismo:

- `pivot` es el nodo completo que empieza en `8`.
- `parent.der = pivot.izq` mueve el subárbol izquierdo de `8` hacia la derecha de `parent`.
- `pivot.izq = parent` coloca a `parent` como hijo izquierdo de `8`.

Por eso, al pensar la rotación, no te enfoques en “mover valores”, sino en “reconectar subárboles completos”.

```python
def rotate_left(self, parent):
    pivot = parent.der
    parent.der = pivot.izq
    pivot.izq = parent
    return pivot
```

En palabras simples: la rotación cambia quién queda arriba, pero conserva todos los subárboles completos.

#### Qué cambia exactamente en `rotate_left`


cada parte significa esto:

- `parent` = el nodo que está arriba y que se va a bajar.
- `pivot` = el hijo derecho de `parent`, el que sube.
- `parent.der = pivot.izq` = el subárbol izquierdo de `pivot` pasa a ser el hijo derecho de `parent`.
- `pivot.izq = parent` = el nodo `parent` pasa a ser el hijo izquierdo de `pivot`.
- `return pivot` = la nueva raíz de esa parte del árbol es `pivot`.

Es decir, el nodo `pivot` ocupa el lugar de `parent`, y `parent` baja un nivel.

#### Regla práctica para no equivocarte

Piensa siempre así:

- `p.info` = valor del nodo
- `p.izq` = árbol izquierdo completo
- `p.der` = árbol derecho completo
- `p` mismo = árbol completo que empieza en ese nodo

Entonces, cuando reestructures, debes reconstruir los enlaces entre nodos y subárboles, no solo los valores.

#### Forma mental útil

Un árbol se reconstruye así:

```python
nuevo_nodo = Nodo(info, izq, der)
```

donde:

- `info` es el valor del nodo
- `izq` es otro árbol
- `der` es otro árbol

Si quieres “subir” un hijo a la posición del padre, lo que debes hacer es conectar el padre con ese subárbol completo:

```python
padre.hijo = hijo_subarbol
```

y no con algo como:

```python
padre.hijo = hijo_subarbol.info
```

Si esta idea queda clara, casi todos los errores de reestructuración desaparecen: el árbol se mueve como una unidad completa, no como un simple dato.
