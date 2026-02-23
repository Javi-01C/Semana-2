

## 1. Decisiones de Diseño y Justificaciones Refinadas

### Decisión 1: Capacidad inicial de 10
* **Justificación original:** Es un número redondo y pequeño para empezar.
* **Justificación refinada:** Un valor de 10 es un buen "término medio" para aplicaciones de propósito general. Evita hacer redimensionamientos inmediatos si el usuario solo agrega un par de elementos, pero no desperdicia demasiada memoria si el arreglo queda casi vacío.
* **Escenario donde la cambiaría:** En un sistema de Big Data donde sé de antemano que voy a procesar lotes de millones de registros, iniciar en 10 provocaría docenas de redimensionamientos inútiles al principio. Ahí la cambiaría a una capacidad inicial de `10,000` o `100,000`.

### Decisión 2: Factor de crecimiento de 2x (Duplicación)
* **Justificación original:** Las matemáticas dicen que garantiza tiempo $O(1)$ amortizado.
* **Justificación refinada:** El factor `2x` prioriza la **velocidad extrema** sobre el ahorro de memoria. Garantiza que las operaciones costosas de copia sean logarítmicamente raras.
* **Escenario donde la cambiaría:** En aplicaciones móviles o sistemas con RAM limitada. Duplicar un arreglo de 500MB pediría 1GB total, desperdiciando hasta 500MB si solo quería agregar un elemento más. En ese caso, usaría un factor de `1.5x` (como hace Java internamente) para un crecimiento más conservador.

### Decisión 3: No implementar reducción de capacidad (Sin *Shrink*)
* **Justificación original:** Mantiene el código simple y evita bugs.
* **Justificación refinada:** Evita el problema del *thrashing* (donde el arreglo crece y se reduce constantemente en el límite, destruyendo el rendimiento). Es aceptable para scripts que se ejecutan y mueren rápido, liberando la memoria al final.
* **Escenario donde la cambiaría:** En un servidor web 24/7 (Daemon). Si el arreglo crece para manejar un pico de tráfico y luego el tráfico baja, esa memoria extra quedará bloqueada para siempre (Memory Bloat). Ahí implementaría una reducción.


### Decisión 4: Lanzar excepción si el índice está fuera de rango
* **Justificación original:** Evita que el programa lea "basura" en la memoria.
* **Justificación refinada:** Sigue el principio de *Fail-Fast* (fallar rápido). Es mejor que el programa colapse con un error claro de `IndexOutOfBounds` a que continúe operando silenciosamente con datos corruptos.
* **Escenario donde la cambiaría:** En un motor de videojuegos en tiempo real. Lanzar y atrapar excepciones es computacionalmente lento y podría causar tirones (lag). Preferiría devolver un código de error predefinido o ignorar la operación para no interrumpir el ciclo de 60 FPS.

### Decisión 5: Copiar elementos con un bucle `for`
* **Justificación original:** Es fácil de leer e implementar.
* **Justificación refinada:** Es una solución agnóstica del lenguaje que funciona bien para tipos de datos simples o cuando estamos aprendiendo la lógica base.
* **Escenario donde la cambiaría:** En software de alto rendimiento en C++. Reemplazaría el bucle `for` por funciones de bajo nivel como `std::memmove` o `std::memcpy`, las cuales están optimizadas a nivel de hardware para mover bloques enteros de memoria en una fracción del tiempo.

----

## 2. Matriz de Contexto vs. Configuración Ideal

| Contexto del Sistema | Capacidad Inicial | Factor Crecimiento | Política de Reducción (*Shrink*) | Método de Copia | Prioridad Principal |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **App de Escritorio (General)** | 10 a 16 | 1.5x - 2.0x | Solo manual (`trimToSize`) | Bucle estándar | Balance general |
| **Microservicio Cloud (24/7)** | 100 | 1.5x | **Sí** (Si uso < 25%, reducir a la mitad) | Copia de bloques | Prevenir fugas de RAM |
| **Sensor IoT (Embebido 512KB)** | 2 a 4 | 1.2x - 1.5x | **Sí** (Agresiva para ahorrar bytes) | Bucle estándar | Conservación de Memoria |
| **Servidor Big Data (Terabytes)** | 10,000+ | 2.0x | No (La memoria sobra, el tiempo no) | Hardware / `memcpy` | Velocidad Absoluta |

---

## 3. Evolución: La decisión que cambiaría de mi código original

Si tuviera que refactorizar mi implementación original (Fase 2) para subirla a un entorno de producción real, cambiaría la decisión de no tener reducción de capacidad (implementaría el método *shrink*).

 *¿Por qué?*
Aprender a reservar memoria con "new" o "malloc" es solo la mitad del trabajo de un buen ingeniero; la otra mitad es ser un buen "ciudadano" del sistema operativo y devolver los recursos que ya no se necesitan. 

Agregaría una regla lógica en el método `eliminar()`: 
Si después de eliminar un elemento, el `tamaño` actual es menor o igual a una cuarta parte (25%) de la `capacidad` total, redimensionaré el arreglo a la mitad (50%). 

Esto protege al sistema del temido *thrashing* (porque hay un margen amplio entre el 25% y el 50%), pero garantiza que un arreglo que tuvo 1 millón de elementos y ahora tiene 10, no secuestre gigabytes de RAM inútilmente.
