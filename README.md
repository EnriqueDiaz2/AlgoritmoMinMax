# Algoritmo Minimax - Juego del Gato

Implementacion del algoritmo **minimax** (con optimizacion **alfa-beta**) aplicado al juego
del gato o tres en linea (3x3), de modo que una persona pueda jugar contra la computadora.

Todo el codigo esta en un solo Jupyter Notebook: **[`minimax_gato.ipynb`](minimax_gato.ipynb)**

---

## Requisitos de la actividad

| # | Requisito | Donde se cumple |
|---|-----------|-----------------|
| 1 | Una persona juega contra la computadora | Seccion 7 (consola) y 8 (interfaz grafica) |
| 2 | Solo se permiten movimientos validos | `jugar()` valida y lanza `ValueError` |
| 3 | Se respetan los turnos de cada jugador | Los bucles de juego alternan estrictamente |
| 4 | La computadora **nunca pierde** | Demostrado en la seccion 9 |

---

## Como ejecutarlo

### En Google Colab (recomendado)

1. Abre `minimax_gato.ipynb` en Colab.
2. `Runtime > Run all` para ejecutar las celdas en orden.
3. Para la interfaz grafica, en una celda nueva ejecuta `!pip install ipywidgets`
   y vuelve a ejecutar la seccion 8.

### En Jupyter local

```bash
pip install notebook ipywidgets
jupyter notebook
```

Abre el archivo y ejecuta las celdas en orden (`Cell > Run All`).

### Solo para jugar, sin interfaz grafica

La seccion 7 trae una version por consola. Descomenta la linea `jugar_texto(X)`
y la computadora responde con minimax. Escribe el numero de la casilla (1 a 9).

---

## Que hace el algoritmo

Minimax construye el arbol de juego completo y lo etiqueta de abajo hacia arriba:

* En los nodos de **MAX** (la computadora) se queda con el **maximo** de los hijos.
* En los nodos de **MIN** (la persona) se queda con el **minimo** de los hijos.

El valor de cada hoja se mide desde el punto de vista de la computadora:

| Situacion | Valor |
|-----------|-------|
| Gano la computadora | `10 - profundidad` |
| Gano la persona | `profundidad - 10` |
| Empate | `0` |

El termino `profundidad` hace que la computadora no se conforme con una victoria segura si
puede conseguirla antes.

La **poda alfa-beta** recorta las ramas que ya no pueden mejorar la decision, reduciendo el
recorrido **26.4x** sin cambiar el resultado.

---

## Resultados verificados

Todos estos numeros se comprueban al ejecutar las celdas 5 y 9 del notebook:

| Prueba | Resultado |
|--------|-----------|
| Alfa-beta vs minimax en las 4,520 posiciones alcanzables | **0 divergencias** |
| Nodos evaluados por minimax | 549,946 |
| Nodos evaluados por alfa-beta | 20,866 |
| Posiciones donde la computadora pierde | **0** |
| Partidas en las que gana la persona | **0** |

Ademas se enumeran las 569 partidas posibles con cada turno inicial del humano, y las
4,520 posiciones alcanzables del tablero, comprobando que la computadora:

1. elige siempre su mejor movimiento,
2. no desperdicia ninguna victoria asegurada,
3. no acepta ninguna derrota evitable.

---

## Un detalle sobre el resultado del juego

Con juego perfeito de ambos lados el gato es **empate**: el valor minimax del tablero vacio
es `0`, no `10`. El que juega primero **no puede forzar la victoria**, pero tampoco puede
perder. El primero gana mas partidas unicamente cuando el rival juega mal.