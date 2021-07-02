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

.. _Gcdex:

Gcdex
~~~~~~~~

- ``Gcdex(m, n)`` (function)

devuelve un registro ``g`` que describe el máximo común divisor extendido de :math:`m` y :math:`n`. El componente ``gcd`` es este ``mcd``, los componentes ``coeff1`` y ``coeff2`` son cofactores enteros tales que ``g.gcd = g.coeff1 * m + g.coeff2 * n``, y los componentes ``coeff3`` y ``coeff4`` son cofactores enteros tales que ``0 = g.coeff3 * m + g.coeff4 * n``.

Si :math:`m` y :math:`n` son distintos de cero, ``AbsInt(g.coeff1)`` es menor o igual que ``AbsInt(n) / (2 * g.gcd)``, y ``AbsInt(g.coeff2)`` es menor o igual que ``AbsInt(m) / (2 * g.gcd)``.

Si :math:`m` o :math:`n` o ambos son cero, ``coeff3`` es ``-n / g.gcd`` y ``coeff4`` es ``m/g.gcd``.

Los coeficientes siempre forman una matriz unimodular, es decir, el determinante ``g.coeff1 * g.coeff4 - g.coeff3 * g.coeff2`` es :math:`1` o :math:`-1`.

.. code-block:: gap
    
    gap> Gcdex( 123, 66 );
    rec( coeff1 := 7, coeff2 := -13, coeff3 := -22, coeff4 := 41, gcd := 3 )

Esto significa ``3 = 7 ∗ 123-13 ∗ 66``, ``0 = −22 ∗ 123 + 41 ∗ 66``.

.. code-block:: gap
    
    gap> Gcdex( 0, -3 );
    rec( coeff1 := 0, coeff2 := -1, coeff3 := 1, coeff4 := 0, gcd := 3 )
    gap> Gcdex( 0, 0 );
    rec( coeff1 := 1, coeff2 := 0, coeff3 := 0, coeff4 := 1, gcd := 0 )

``GcdRepresentation`` proporciona una funcionalidad similar sobre anillos euclidianos arbitrarios.

.. _LcmInt:


LcmInt
~~~~~~

- ``LcmInt(m, n)`` (function)

devuelve el mínimo común múltiplo de los enteros :math:`m` y :math:`n`.

LcmInt_ es un método utilizado por la operación general ``Lcm``.

.. code-block:: gap
    
    gap> LcmInt( 123, 66 );
    2706

.. _CoefficientsQadic:

CoefficientsQadic
~~~~~~~~~~~~~~~~~~

- ``CoefficientsQadic(i, q)`` (operation)

devuelve la representación :math:`q`-ádica del entero :math:`i` como una lista ``l`` de coeficientes que satisfacen la igualdad

.. math::
    
    i = \sum_{j = 0} q^{j} \cdot l [j + 1]
    
para un número entero :math:`q> 1`.

.. code-block:: gap
    
    gap> l:=CoefficientsQadic(462,3);
    [ 0, 1, 0, 2, 2, 1 ]



.. _CoefficientsMultiadic:

CoefficientsMultiadic
~~~~~~~~~~~~~~~~~~~~~

- ``CoefficientsMultiadic(ints, int)`` (function)

devuelve la expansión multiádica del entero ``int`` módulo los enteros dados en ``ints`` (en orden ascendente). Devuelve una lista de coeficientes en orden inverso al de ``ints``.

.. _ChineseRem:

ChineseRem
~~~~~~~~~~~

- ``ChineseRem(moduli, residues)`` (function)

ChineseRem_ devuelve la combinación de los ``residues`` módulo los ``moduli``, es decir, el entero único ``c`` de ``[0..Lcm(moduli )-1]`` tal que ``c = residues [i]`` módulo ``moduli [i]`` para todo ``i``, si existe.

Si no existe tal combinación, ChineseRem_ indica un error.

Tal combinación existe si y sólo si ``residues [i] = residues [k] mod Gcd( moduli [i], moduli [k] )`` para cada par ``i``, ``k``. Tenga en cuenta que esto implica que existe tal combinación si los módulos son pares relativamente primos. Esto se llama teorema del resto chino.

.. code-block:: gap
    
    gap> ChineseRem( [ 2, 3, 5, 7 ], [ 1, 2, 3, 4 ] );
    53
    gap> ChineseRem( [ 6, 10, 14 ], [ 1, 3, 5 ] );
    103

.. code-block:: gap

    gap> ChineseRem( [ 6, 10, 14 ], [ 1, 2, 3 ] );
    Error, the residues must be equal modulo 2 at /proc/cygdrive/C/gap-4.10.2/lib/integer.gi:408 called from
    <function "ChineseRem">( <arguments> )
     called from read-eval loop at *stdin*:1
    you can 'quit;' to quit to outer loop, or
    you can 'return;' to continue
    brk>


.. _PowerModInt:

PowerModInt
~~~~~~~~~~~

- ``PowerModInt(r, e, m)`` (function)

devuelve :math:`r^{e} (\texttt{ mod } m)` para enteros ``r``, ``e`` y ``m``.

Tenga en cuenta que PowerModInt_ puede reducir los resultados intermedios y, por lo tanto, generalmente será más rápido que usar ``r^e mod m``, que calcularía :math:`r^{e}` primero y reduciría el resultado después.

PowerModInt_ es un método para el funcionamiento general ``PowerMod``.

Enteros Primos y Factorización
-------------------------------------

.. _Primes:

Primes
~~~~~~~~

- ``Primes`` (global variable)

Primes es una lista estrictamente ordenada de los :math:`168` números primos inferiores a :math:`1000`.

Esto se usa en :ref:`IsProbablyPrimeInt` y ``FactorsInt (14.4.7)`` para lanzar pequeños números primos rápidamente.

.. code-block:: gap
    
    gap> Primes;
    [ 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97,
      101, 103, 107, 109, 113, 127, 131, 137, 139, 149, 151, 157, 163, 167, 173, 179, 181, 191, 193, 
      197, 199, 211, 223, 227, 229, 233, 239, 241, 251, 257, 263, 269, 271, 277, 281, 283, 293, 307,
      311, 313, 317, 331, 337, 347, 349, 353, 359, 367, 373, 379, 383, 389, 397, 401, 409, 419, 421,
      431, 433, 439, 443, 449, 457, 461, 463, 467, 479, 487, 491, 499, 503, 509, 521, 523, 541, 547,
      557, 563, 569, 571, 577, 587, 593, 599, 601, 607, 613, 617, 619, 631, 641, 643, 647, 653, 659,
      661, 673, 677, 683, 691, 701, 709, 719, 727, 733, 739, 743, 751, 757, 761, 769, 773, 787, 797,
      809, 811, 821, 823, 827, 829, 839, 853, 857, 859, 863, 877, 881, 883, 887, 907, 911, 919, 929,
      937, 941, 947, 953, 967, 971, 977, 983, 991, 997 ]
    gap> Primes[1];
    2
    gap> Primes[100];
    541




.. _IsProbablyPrimeInt:

IsPrimeInt
~~~~~~~~~~~

- ``IsProbablyPrimeInt(n)`` (function)

- ``IsProbablyPrimeInt(n)`` (function)

IsProbablyPrimeInt_ devuelve ``false`` si puede probar que el entero :math:`n` es compuesto y ``true`` en caso contrario.

Por convención ``IsProbablyPrimeInt(0) = IsProbablyPrimeInt(1) = false`` y definimos ``IsProbablyPrimeInt(-n) = IsProbablyPrimeInt(n)``.

:ref:`IsProbablyPrimeInt` devolverá ``true`` para cada primo :math:`n`. :ref:`IsProbablyPrimeInt` devolverá falso para todos los compuestos :math:`n <10^{18}` y para todos los compuestos :math:`n` que tengan un factor :math:`p <1000`. Por lo tanto, para los números enteros :math:`n <10^{18}`, :ref:`IsProbablyPrimeInt` es una prueba de primalidad adecuada. Es concebible que :ref:`IsProbablyPrimeInt` pueda devolver ``true`` para algún compuesto :math:`n <10^{18}`, pero actualmente no se conoce tal :math:`n`. Entonces, para enteros :math:`n < 10^{18}`, :ref:`IsProbablyPrimeInt` es una prueba de primaria probable. :ref:`IsProbablyPrimeInt` emitirá una advertencia cuando su argumento sea probablemente principal pero no probado. (La función IsProbablyPrimeInt_ hará un cálculo similar pero no emitirá una advertencia).

La advertencia puede desactivarse mediante ``SetInfoLevel(InfoPrimeInt, 0);`` , el nivel predeterminado es :math:`1` (consulte también SetInfoLevel (7.4.3)).

Si existen compuestos que engañan a :ref:`IsProbablyPrimeInt`, serían extremadamente raros, y encontrar uno por pura casualidad podría ser menos probable que encontrar un error en GAP. Agradeceríamos que se nos informara sobre cualquier ejemplo de un número compuesto :math:`n` para el que :ref:`IsProbablyPrimeInt` devuelve ``true``.

:ref:`IsProbablyPrimeInt` es un algoritmo determinista, es decir, los cálculos no involucran números aleatorios y las llamadas repetidas siempre devolverán el mismo resultado. :ref:`IsProbablyPrimeInt` primero hace divisiones de prueba entre los números primos menores que :math:`1000`. Luego, prueba que :math:`n` es un pseudoprime fuerte w.r.t. la base :math:`2`. Finalmente, prueba si :math:`n` es un pseudoprime w.r.t. de Lucas. el no residuo cuadrático más pequeño de :math:`n`. Se puede encontrar una mejor descripción en el comentario en el archivo de biblioteca ``primality.gi``.

El tiempo que tarda :ref:`IsProbablyPrimeInt` es aproximadamente proporcional a la tercera potencia del número de dígitos de :math:`n`. Probar números con varios cientos de dígitos es bastante factible.

:ref:`IsProbablyPrimeInt` es un método para la operación general ``IsPrime``.

.. note::
    
    En futuras versiones de GAP, esperamos cambiar la definición de :ref:`IsProbablyPrimeInt` para que devuelva ``true`` solo para números primos probados (actualmente, carecemos de una función de prueba de primalidad suficientemente buena). En las aplicaciones, use explícitamente :ref:`IsProbablyPrimeInt` o IsProbablyPrimeInt_ con este cambio en mente.


.. code-block:: gap
    
    gap> IsPrimeInt( 2^31 - 1 );
    true
    gap> IsPrimeInt( 10^42 + 1 );
    false

.. _PrimalityProof:

PrimalityProof
~~~~~~~~~~~~~~~

- ``PrimalityProof(n)`` (function)


Construya una prueba verificable por máquina de la primalidad de (el primo probable) :math:`n`, siguiendo las ideas de [BLS75]. La prueba consta de varias pruebas de pseudoprimalidad de Fermat y Lucas, que tomadas en su conjunto prueban la primacía. La prueba se representa como una lista de testigos de dos tipos. El primer tipo, ``["F", divisor, base]``, indica una prueba de pseudoprimalidad de Fermat exitosa, donde :math:`n` es un pseudoprime fuerte en la base con orden no divisible por ``(n - 1) / divisor``. El segundo tipo, ``[ "L", divisor, discriminante, P ]`` indica una prueba de pseudoprimalidad de Lucas exitosa, para una forma cuadrática del discriminante dado y el término medio ``P`` con una verificación adicional en ``(n + 1) / divisor.``