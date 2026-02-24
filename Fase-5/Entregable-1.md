
---
 ***1. Tabla Comparativa de Lenguajes***

| **Lenguaje** | **Estructura Base** | **Capacidad Inicial** | **Factor Crecimiento** | **Característica Única de Diseño** |
| :--- | :--- | :--- | :--- | :--- |
| Java | ArrayList | 10 (Asignación perezosa) | **1.5x** | Usa métodos nativos de C++ ( System.arraycopy ) para mover bloques de memoria masivos directamente en el hardware. |
| Python | list | 0 (Vacía) | **~1.125x** (Adaptativo) | No guarda valores, guarda *punteros* a objetos. Por eso puedes mezclar tipos de datos ([1, "a", True]), sacrificando rendimiento en caché. |
| C++ | std::vector | 0 (Depende del compilador) | **1.5x o 2x** | Utiliza Move Semantics. En lugar de copiar objetos pesados al redimensionar, "roba" sus recursos en memoria, haciéndolo increíblemente rápido. |
| JavaScript | Array (Motor V8) | Dinámica | Dinámico | No son arreglos reales, son Diccionarios/Hash Maps disfrazados. El motor hace optimizaciones extremas en tiempo real para simular arreglos de C++. |

---

***2. La Decisión de Diseño más Interesante***

La decisión arquitectónica más fascinante es cómo el Motor V8 de Google (JavaScript) maneja los arreglos de forma invisible para el programador. 
El motor intenta tratar tu arreglo JS como un arreglo contiguo ultrarrápido de C++ (llamado Packed Array). Sin embargo, si cometes el error de dejar un "hueco" en los índices (por ejemplo, tienes 3 elementos y haces `arreglo[10] = "Hola"`), el motor entra en pánico por el desperdicio de RAM y degrada tu arreglo permanentemente a un Diccionario/Tabla Hash (llamado **Holey Array**). 

>¿Por qué es relevante? Porque nos enseña que en lenguajes de alto nivel, la pereza al codificar destruye el rendimiento. Un arreglo "Holey" es muchísimo más lento de leer y escribir. Entender esto te hace escribir código JavaScript mucho más optimizado que el desarrollador promedio.

---

 ****3. Evolución: ¿Cómo cambiaría mi propia implementación?****

Después de esta investigación empírica e industrial, si tuviera que reescribir la clase ArregloDinamico que hice en la Fase 2, aplicaría dos cambios fundamentales basados en C++ y Java:

1. Cambiaría el factor de crecimiento a 1.5x: Descubrimos en nuestros benchmarks que 2x es rápido pero desperdicia hasta el 50% de la memoria al redimensionar.
    Java y el compilador de Windows para C++ usan 1.5x porque es el "punto dulce" perfecto entre velocidad amortizada $O(1)$ y responsabilidad con la memoria RAM del servidor.
2. Expondría el control de memoria al usuario: Agregaría métodos explícitos como el reserve(capacidad) y shrink_to_fit() de C++. Si el usuario sabe que va a cargar un millón de registros de una base de datos,
    debería poder decirle al arreglo que reserve ese millón de espacios desde el inicio, evitando decenas de costosos redimensionamientos automáticos.


   
