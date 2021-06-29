Objetos y Elementos
====================

Objects
---------------------------

Casi todas las cosas con las que uno se ocupa en GAP son objetos. Por ejemplo, un número entero es un objeto, al igual que una lista de números enteros, una matriz, una permutación, una función, una lista de funciones, un registro, un grupo, una clase lateral o una clase de conjugación en un grupo.

Ejemplos de cosas que no son objetos son comentarios que son solo construcciones léxicas, mientras que los bucles que son solo construcciones sintácticas y expresiones, como ``1 + 1``; pero tenga en cuenta que el valor de una expresión, en este caso el entero ``2``, es un objeto.

Los objetos se pueden asignar a variables y todo lo que se puede asignar a una variable es un objeto.

De manera análoga, los objetos pueden usarse como argumentos de funciones y pueden ser devueltos por funciones.

.. _IsObject:

IsObject
~~~~~~~~~~

- ``IsObject(obj)`` (Category)

IsObject devuelve verdadero si el objeto obj es un objeto. Obviamente, nunca puede devolver falso.

Se puede usar como filtro en InstallMethod (78.2.1) cuando uno de los argumentos puede ser cualquier cosa.

Elementos como clases de equivalencia
--------------------------------------

La operación de igualdad "``=``" define una relación de equivalencia en todos los objetos GAP. Las clases de equivalencia se denominan *elementos*.

Básicamente, hay tres razones para considerar iguales los diferentes objetos. En primer lugar, la misma información puede almacenarse en diferentes lugares. En segundo lugar, la misma información puede almacenarse de diferentes formas; por ejemplo, un polinomio se puede almacenar escasa o densamente. En tercer lugar, la información diferente puede ser igual módulo a una relación de equivalencia matemática. Por ejemplo, en un grupo presentado finitamente con la relación :math:`a^{2} = 1` los diferentes objetos :math:`a` y :math:`a^{3}` describen el mismo elemento