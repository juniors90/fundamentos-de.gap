Enteros
=======



Uno de los tipos de datos más fundamentales en todos los lenguajes de programación es el tipo entero. GAP no es una excepción.

Los números enteros GAP se ingresan como una secuencia de dígitos decimales opcionalmente precedidos por un signo "``+``" para números enteros positivos o un signo "``-``" para números enteros negativos. El tamaño de los números enteros en GAP solo está limitado por la cantidad de memoria disponible, por lo que puede calcular con números enteros que tengan miles de dígitos.

.. code-block:: gap

    gap> -1234;
    -1234
    gap> 123456789012345678901234567890123456789012345678901234567890;
    123456789012345678901234567890123456789012345678901234567890


Integers: Variables Globales
-----------------------------

Integers (variable global )
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- ``Integers`` (global variable)
- ``PositiveIntegers`` (global variable)
- ``NonnegativeIntegers`` (global variable)

Estas variables globales representan el anillo de números enteros y los semirigidos de números enteros positivos y no negativos, respectivamente.

.. code-block::
    
    gap> Size( Integers ); 2 in Integers;
    infinity
    true

Enteros es un subconjunto de :doc:`17-racionales`, que es un subconjunto de :doc:`18-ciclotomics`.