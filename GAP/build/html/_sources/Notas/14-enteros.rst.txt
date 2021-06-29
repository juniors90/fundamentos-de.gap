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

.. _Integers:

Integers (variable global )
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- ``Integers`` (global variable)

- ``PositiveIntegers`` (global variable)

- ``NonnegativeIntegers`` (global variable)

Estas variables globales representan el anillo de números enteros y los semirigidos de números enteros positivos y no negativos, respectivamente.

.. code-block:: gap
    
    gap> Size( Integers ); 2 in Integers;
    infinity
    true

Enteros es un subconjunto de :doc:`17-racionales`, que es un subconjunto de :doc:`18-ciclotomics`.

IsIntegers
~~~~~~~~~~~~~~~~~

- ``IsIntegers(obj)`` (Category)

- ``IsPositiveIntegers(obj)`` (Category)

- ``IsNonnegativeIntegers(obj)`` (Category)

son las categorías que definen los dominios ``Integers``, ``PositiveIntegers`` y ``NonnegativeIntegers`` dadas en Integers_.

.. code-block:: gap
    
    gap> IsIntegers?
    <Category "IsIntegers">
    gap> IsIntegers( Integers ); IsIntegers( Rationals ); IsIntegers( 7 );
    true
    false
    false


Operaciones elementales para enteros
------------------------------------

.. _IsInt:

IsInt
~~~~~

- ``IsInt(obj)`` (Category)

Cada entero racional se encuentra en la categoría IsInt, que es una subcategoría de ``IsRat``.

IsPosInt
~~~~~~~~

- IsPosInt(obj) (Category)

Every positive integer lies in the category IsPosInt.

.. _Int:

Int
~~~

- ``Int(elm)`` (attribute)

Int_ devuelve un entero ``int`` cuyo significado depende del tipo de ``elm``.

**Por ejemplo:**

- Si ``elm`` es un número racional (Ver :doc:`17-racionales`), entonces ``int`` es la parte entera del cociente del numerador y el denominador de elm (consulte ``QuoInt`` (14.3.1)).

- Si ``elm`` es un elemento de un campo primo finito entonces ``int`` es el entero no negativo más pequeño tal que ``elm = int * One (elm)``.

- Si ``elm`` es una cadena (Ver :doc:`27-strings`) que consta en su totalidad de dígitos decimales ``"0"``, ``"1"``,. . ., ``"9"`` y, opcionalmente, un signo "``-``" (en la primera posición), entonces ``int`` es el número entero descrito por esta cadena. Para todas las demás cadenas, se devuelve :ref:`fail`. Ver ``Int`` (27.9.1).

La operación ``String`` (27.7.6) se puede utilizar para calcular una cadena para enteros racionales, de hecho para todas las ciclotómicas.

.. code-block:: gap

    gap> # Vemos que es un atributo
    gap> Int?
    <Attribute "Int">
    gap> # como parte entera 
    gap> Int( 4/3 ); Int( -2/3 );
    1
    0
    gap> # elm = Z(5) es un elemento de un campo primo finito
    gap> int:= Int( Z(5) ); int * One( Z(5) );
    2
    Z(5)
    gap> # si elm es una cadena
    gap> Int( "12345" ); Int( "-27" ); Int( "-27/3" );
    12345
    -27
    fail


IsEvenInt
~~~~~~~~~~~~

- ``IsEvenInt(n)`` (function)

comprueba si el número entero :math:`n` es divisible por :math:`2`.

IsOddInt
~~~~~~~~~

- ``IsOddInt(n)`` (function)

comprueba si el número entero :math:`n` no es divisible por :math:`2`.

.. _AbsInt:

AbsInt
~~~~~~~

- ``AbsInt(n)`` (function)

AbsInt_ devuelve el valor absoluto del número entero :math:`n`, es decir, :math:`n` si :math:`n` es positivo, :math:`-n` si :math:`n` es negativo y :math:`0` si :math:`n` es :math:`0`.

AbsInt_ es un caso especial de la operación general ``EuclideanDegree`` (56.6.2). Consulte también :ref:`AbsoluteValue`.

.. code-block:: gap

    gap> AbsInt( 33 );
    33
    gap> AbsInt( -214378 );
    214378
    gap> AbsInt( 0 );
    0

.. _SignInt:

SignInt
~~~~~~~~

- SignInt(n) (function)

SignInt devuelve el signo del número entero :math:`n`, es decir, :math:`1` si :math:`n` es positivo, :math:`-1` si :math:`n` es negativo y :math:`0` si :math:`n` es :math:`0`.

.. code-block:: gap
    
    gap> SignInt( 33 );
    1
    gap> SignInt( -214378 );
    -1
    gap> SignInt( 0 );
    0

.. _LogInt:

LogInt
~~~~~~~

- ``LogInt(n, base)`` (function)

LogInt_ devuelve la parte entera del logaritmo del entero positivo :math:`n` con respecto a la base del entero positivo, es decir, el entero positivo más grande :math:`e` tal que :math:`base^{e} \leq n`. La función LogInt_ señalará un error si :math:`n` o :math:`base` no son positivos.

Para ``base = 2`` esto es muy eficiente porque se usa la representación binaria interna del entero.

.. code-block:: gap
    
    gap> LogInt( 1030, 2 );
    10
    gap> 2^10;
    1024
    gap> LogInt( 1, 10 );
    0

.. _RootInt:

RootInt
~~~~~~~~

- ``RootInt(n[, k])`` (function)

RootInt_ devuelve la parte entera de la :math:`k`-ésima raíz del entero :math:`n`. Si no se proporciona el argumento de entero opcional :math:`k`, el valor predeterminado es :math:`2`, es decir, RootInt_ devuelve la parte entera de la raíz cuadrada en este caso.

- Si :math:`n` es positivo, RootInt_ devuelve el mayor entero positivo :math:`r` tal que :math:`r^{k} \leq n`.

- Si :math:`n` es negativo y :math:`k` es impar, RootInt_ devuelve ``-RootInt (-n, k)``.

- Si :math:`n` es negativo y :math:`k` es par, RootInt_ provocará un error. RootInt_ también provocará un error si :math:`k` es :math:`0` o negativo.

.. code-block:: gap
    
    gap> RootInt( 361 );
    19
    gap> RootInt( 2 * 10^12 );
    1414213
    gap> RootInt( 17000, 5 );
    7
    gap> 7^5;
    16807

.. _SmallestRootInt:

SmallestRootInt
~~~~~~~~~~~~~~~~

- ``SmallestRootInt(n)`` (function)

SmallestRootInt_ devuelve la raíz más pequeña del entero :math:`n`.

La raíz más pequeña de un número entero :math:`n` es el número entero :math:`r` de valor absoluto más pequeño para el que existe un número entero positivo :math:`k` tal que :math:`n = r^{k}`.

.. code-block:: gap
    
    gap> SmallestRootInt( 2^30 );
    2
    gap> SmallestRootInt( -(2^30) );
    -4

Notar que :math:`(-2)^{30} = +(2^{30})`.

.. code-block:: gap
    
    gap> SmallestRootInt( 279936 );
    6
    gap> LogInt( 279936, 6 );
    7
    gap> SmallestRootInt( 1001 );
    1001

.. _ListOfDigits:

ListOfDigits
~~~~~~~~~~~~

- ``ListOfDigits(n)`` (function)

Para un entero positivo :math:`n`, esta función devuelve una lista ``l``, que consta de los dígitos de n en representación decimal.

.. code-block:: gap
    
    gap> ListOfDigits(3142);
    [ 3, 1, 4, 2 ]

.. _Random-integer:

Random (para Enteros)
~~~~~~~~~~~~~~~~~~~~~

- ``Random(Integers)`` (método)

:ref:`Random-integer` devuelve enteros pseudoaleatorios entre :math:`−10` y :math:`10` distribuidos según una distribución binomial. Para generar enteros distribuidos uniformemente a partir de un rango, use la construcción ``Random( [ low .. high ] )`` (Ver Random (30.7.1))

Cocientes y restos
------------------

.. _QuoInt:

QuoInt
~~~~~~

- ``QuoInt(n, m)`` (function)

QuoInt_ devuelve la parte entera del cociente de sus operandos enteros.

Si :math:`n` y :math:`m` son positivos, QuoInt_ devuelve el mayor entero positivo :math:`q` tal que :math:`q \ast m \leq n`. Si :math:`n` o :math:`m` o ambos son negativos, el valor absoluto de la parte entera del cociente es el cociente de los valores absolutos de :math:`n` y :math:`m`, y su signo es el producto de los signos de :math:`n` y :math:`m`.

QuoInt_ se usa en un método para la operación general ``EuclideanQuotient``.

.. code-block:: gap
    
    gap> QuoInt(5,3);
    1
    gap> QuoInt(-5,3);
    -1
    gap> QuoInt(5,-3);
    -1
    gap> QuoInt(-5,-3);
    1


.. _BestQuoInt:

BestQuoInt
~~~~~~~~~~

- ``BestQuoInt(n, m)`` (function)

BestQuoInt_ devuelve el mejor cociente :math:`q` de los enteros :math:`n` y :math:`m`. Este es el cociente tal que ``n - q ∗ m`` tiene un valor absoluto mínimo. Si hay dos cocientes cuyos residuos tienen el mismo valor absoluto, se elige el cociente con el valor absoluto más pequeño.

.. code-block:: gap
    
    gap> BestQuoInt( 5, 3 );
    2
    gap> BestQuoInt( -5, 3 );
    -2


.. _RemInt:

RemInt
~~~~~~

- ``RemInt(n, m)`` (function)

RemInt devuelve el resto de sus dos operandos enteros.

Si :math:`m` no es igual a cero, RemInt devuelve ``n - m * QuoInt (n, m)``. Tenga en cuenta que las reglas dadas para QuoInt_ implican que el valor de retorno de RemInt_ tiene el mismo signo que :math:`n` y su valor absoluto es estrictamente menor que el valor absoluto de :math:`m`. Tenga en cuenta también que el valor de retorno es igual a :math:`n\text{ mod }m` cuando tanto :math:`n` como :math:`m` no son negativos. Dividir por :math:`0` indica un error.

RemInt se usa en un método para la operación general ``EuclideanRemainder``.

.. code-block:: gap
    
    gap> RemInt(5,3);
    2
    gap> RemInt(-5,3);
    -2
    gap> RemInt(5,-3);
    2
    gap> RemInt(-5,-3);
    -2

.. _GcdInt:

GcdInt
~~~~~~~

- ``GcdInt(m, n)`` (function)

GcdInt_ devuelve el máximo común divisor de sus dos operandos enteros :math:`n` y :math:`m`, es decir, el mayor número entero que divide :math:`n` y :math:`m`. El máximo común divisor nunca es negativo, incluso si los argumentos lo son. Definimos ``GcdInt( m, 0 ) = GcdInt( 0, m ) = AbsInt( m )`` y ``GcdInt( 0, 0 ) = 0``.

GcdInt es un método utilizado por la función general ``Gcd``.

.. code-block:: gap
    
    gap> GcdInt( 123, 66 );
    3