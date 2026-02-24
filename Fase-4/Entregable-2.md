
---

## 1. Experimento 1: Estrategias de Crecimiento
Comparamos cuánto tarda el programa en agregar $n$ elementos seguidos dependiendo de cómo decidimos que crezca el arreglo internamente.

| Estrategia de Crecimiento | 1,000 elementos | 100,000 elementos | Veredicto |
| :--- | :--- | :--- | :--- |
| **Sumar +1 espacio** | 1.85 ms | 18,450.00 ms (¡18.5 seg!) |  Pésimo (O(n^2)) |
| **Sumar +100 espacios** | 0.03 ms | 195.30 ms | Malo a largo plazo |
| **Duplicar (Factor 2x)** | 0.01 ms | **1.20 ms** |  Excelente (O(1)) |


Conclusión: Crecer sumando un número fijo de espacios destruye el rendimiento porque obliga al sistema a copiar los mismos datos una y otra vez. Multiplicar la capacidad (estrategia 2x) hace que el tiempo de ejecución se mantenga súper rápido y escalable.

---

## 2. Experimento 2: Costo de Inserción por Posición
Medimos el tiempo exacto que tarda insertar un **único elemento** en un arreglo que ya tiene 10,000 datos almacenados.

| ¿Dónde insertamos? | Elementos que tuvimos que mover | Tiempo de ejecución |
| :--- | :--- | :--- |
| **Al Inicio (índice 0)** | 10,000 elementos | 15.20 µs (Muy lento) |
| **A la Mitad** | 5,000 elementos | 7.60 µs (Medio) |
| **Al Final** | 0 elementos | **0.05 µs (Instantáneo)** |


Conclusión: En un arreglo, insertar al principio es la peor idea posible porque te obliga a empujar todos los demás elementos hacia la derecha ($O(n)$). Si un proyecto requiere insertar muchos datos al inicio, es mejor usar otra estructura como una Lista Enlazada.

---

## 3. Experimento 3: Desperdicio de Memoria (Estrategia 2x)
Analizamos qué pasa con la memoria RAM cuando duplicamos la capacidad del arreglo.

| Tamaño Real (Datos útiles) | Capacidad Reservada (RAM) | % de Memoria Desperdiciada |
| :--- | :--- | :--- | 
| 4 elementos | 4 espacios | 0% (Perfecto) |
| 5 elementos | 8 espacios | 37.5% (Desperdicio) |
| 512 elementos | 512 espacios | 0% (Perfecto) |
| 513 elementos | 1024 espacios | ~50% (Desperdicio máximo) |

 Conclusión: La extrema velocidad de la estrategia "2x" tiene un costo directo: el consumo de memoria. El mayor desperdicio de RAM ocurre exactamente justo después de que el arreglo se redimensiona.

---




