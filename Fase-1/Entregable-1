             **DIAGRAMA**

Tipo: double (8 bytes).
Dirección Base: 0x5000.
Arreglo: precios = [10.50, 20.75, 99.99, 5.00]
      Dirección = Base + (Índice x Tamaño_Byte)
         El Cálculo: Dirección = 5000 + (2 x8)
                     Dirección = 5000 + 16
                     Dirección = 5016


        **TABLA COMPARATIVA**
 _____________________________________________________________________________________________
| Operación             | Escenario          | Elementos que se mueven (Costo)   | Complejidad|
| Leer/Escribir         |arr[5] = x          |0                                  |O(1)        |
| Insertar              |Al Final (arr[10])  |0                                  |O(1)        |
| Insertar              |Al Inicio (arr[0])  |10 (Todos se recorren)             |O(n)        |
| Insertar              |Al Medio (arr[5])   |5 (La mitad se recorre)            |O(n)        |
| Eliminar              |Al Final (arr[9])   |0                                  |O(1)        |
| Eliminar              |Al Inicio (arr[0])  |9 (Los restantes se recorren)      |O(n)        |
|_____________________________________________________________________________________________|



           -- **Por qué insertar al inicio es más costoso que insertar al final?**--

Insertar al inicio es la operación más cara posible en un arreglo debido a la propiedad fundamental de Memoria Contigua.
En un arreglo, los elementos deben estar físicamente uno al lado del otro, sin huecos.
Si quiero poner un nuevo valor en la posición 0, el valor que estaba ahí no puede desaparecer. Debe moverse a la posición 1.
Pero la posición 1 estaba ocupada, así que ese valor debe moverse a la 2.
Este efecto dominó continúa hasta el final del arreglo.


