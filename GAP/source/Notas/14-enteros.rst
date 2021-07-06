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

- ``IsIntegers(obj)`` (Categoría)

- ``IsPositiveIntegers(obj)`` (Categoría)

- ``IsNonnegativeIntegers(obj)`` (Categoría)

son las categorías que definen los dominios ``Integers``, ``PositiveIntegers`` y ``NonnegativeIntegers`` dadas en Integers_.

.. code-block:: gap
    
    gap> IsIntegers?
    <Categoría "IsIntegers">
    gap> IsIntegers( Integers ); IsIntegers( Rationals ); IsIntegers( 7 );
    true
    false
    false


Operaciones elementales para enteros
------------------------------------

.. _IsInt:

IsInt
~~~~~

- ``IsInt(obj)`` (Categoría)

Cada entero racional se encuentra en la categoría IsInt, que es una subcategoría de ``IsRat``.

IsPosInt
~~~~~~~~

- IsPosInt(obj) (Categoría)

Every positive integer lies in the Categoría IsPosInt.

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

- ``IsEvenInt(n)`` (función)

comprueba si el número entero :math:`n` es divisible por :math:`2`.

IsOddInt
~~~~~~~~~

- ``IsOddInt(n)`` (función)

comprueba si el número entero :math:`n` no es divisible por :math:`2`.

.. _AbsInt:

AbsInt
~~~~~~~

- ``AbsInt(n)`` (función)

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

- SignInt(n) (función)

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

- ``LogInt(n, base)`` (función)

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

- ``RootInt(n[, k])`` (función)

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

- ``SmallestRootInt(n)`` (función)

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

- ``ListOfDigits(n)`` (función)

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

- ``QuoInt(n, m)`` (función)

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

- ``BestQuoInt(n, m)`` (función)

BestQuoInt_ devuelve el mejor cociente :math:`q` de los enteros :math:`n` y :math:`m`. Este es el cociente tal que ``n - q ∗ m`` tiene un valor absoluto mínimo. Si hay dos cocientes cuyos residuos tienen el mismo valor absoluto, se elige el cociente con el valor absoluto más pequeño.

.. code-block:: gap
    
    gap> BestQuoInt( 5, 3 );
    2
    gap> BestQuoInt( -5, 3 );
    -2


.. _RemInt:

RemInt
~~~~~~

- ``RemInt(n, m)`` (función)

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

- ``GcdInt(m, n)`` (función)

GcdInt_ devuelve el máximo común divisor de sus dos operandos enteros :math:`n` y :math:`m`, es decir, el mayor número entero que divide :math:`n` y :math:`m`. El máximo común divisor nunca es negativo, incluso si los argumentos lo son. Definimos ``GcdInt( m, 0 ) = GcdInt( 0, m ) = AbsInt( m )`` y ``GcdInt( 0, 0 ) = 0``.

GcdInt es un método utilizado por la función general ``Gcd``.

.. code-block:: gap
    
    gap> GcdInt( 123, 66 );
    3

.. _Gcdex:

Gcdex
~~~~~~~~

- ``Gcdex(m, n)`` (función)

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

- ``LcmInt(m, n)`` (función)

devuelve el mínimo común múltiplo de los enteros :math:`m` y :math:`n`.

LcmInt_ es un método utilizado por la operación general ``Lcm``.

.. code-block:: gap
    
    gap> LcmInt( 123, 66 );
    2706

.. _CoefficientsQadic:

CoefficientsQadic
~~~~~~~~~~~~~~~~~~

- ``CoefficientsQadic(i, q)`` (operación)

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

- ``CoefficientsMultiadic(ints, int)`` (función)

devuelve la expansión multiádica del entero ``int`` módulo los enteros dados en ``ints`` (en orden ascendente). Devuelve una lista de coeficientes en orden inverso al de ``ints``.

.. _ChineseRem:

ChineseRem
~~~~~~~~~~~

- ``ChineseRem(moduli, residues)`` (función)

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
    <función "ChineseRem">( <arguments> )
     called from read-eval loop at *stdin*:1
    you can 'quit;' to quit to outer loop, or
    you can 'return;' to continue
    brk>


.. _PowerModInt:

PowerModInt
~~~~~~~~~~~

- ``PowerModInt(r, e, m)`` (función)

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
    gap> # Teorema de Wilson
    gap> RemInt(Factorial(Primes[120]-1)+1, Primes[120]);
    0





.. _IsProbablyPrimeInt:

IsPrimeInt
~~~~~~~~~~~

- ``IsProbablyPrimeInt(n)`` (función)

- ``IsProbablyPrimeInt(n)`` (función)

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

- ``PrimalityProof(n)`` (función)


Construya una prueba verificable por máquina de la primalidad de (el primo probable) :math:`n`, siguiendo las ideas de [BLS75]. La prueba consta de varias pruebas de pseudoprimalidad de Fermat y Lucas, que tomadas en su conjunto prueban la primacía. La prueba se representa como una lista de testigos de dos tipos. El primer tipo, ``["F", divisor, base]``, indica una prueba de pseudoprimalidad de Fermat exitosa, donde :math:`n` es un pseudoprime fuerte en la base con orden no divisible por ``(n - 1) / divisor``. El segundo tipo, ``[ "L", divisor, discriminante, P ]`` indica una prueba de pseudoprimalidad de Lucas exitosa, para una forma cuadrática del discriminante dado y el término medio ``P`` con una verificación adicional en ``(n + 1) / divisor.``


.. _IsPrimePowerInt:

IsPrimePowerInt
~~~~~~~~~~~~~~~~~~~~~~

- ``IsPrimePowerInt(n)`` (función)

IsPrimePowerInt_ devuelve ``true`` si el número entero :math:`n` es una potencia principal y ``false`` en caso contrario.

Un número entero :math:`n` es una potencia prima si existe un número primo :math:`p` y un número entero positivo :math:`i` tal que :math:`p^{i} = n`.

Si :math:`n` es negativo, la condición es que debe existir un primo negativo :math:`p` y un entero positivo impar :math:`i` tal que :math:`p^{i} = n`. Los enteros :math:`1` y :math:`-1` no son poderes primos.

Tenga en cuenta que IsPrimePowerInt_ usa SmallestRootInt_ y una prueba de primalidad probable (consulte IsPrimeInt_).

.. code-block:: gap
    
    gap> IsPrimePowerInt( 31^5 );
    true
    gap> IsPrimePowerInt( 2^31-1 ); # 2^31-1 is actually a prime
    true
    gap> IsPrimePowerInt( 2^63-1 );
    false
    gap> Filtered( [-10..10], IsPrimePowerInt );
    [ -8, -7, -5, -3, -2, 2, 3, 4, 5, 7, 8, 9 ]

.. _NextPrimeInt:

NextPrimeInt
~~~~~~~~~~~~~

- ``NextPrimeInt(n)`` (función)

NextPrimeInt_ devuelve el número primo más pequeño que es estrictamente mayor que el entero :math:`n`. Tenga en cuenta que NextPrimeInt_ utiliza una prueba de primalidad probable (consulte IsPrimeInt_).


.. code-block:: gap
    
    gap> NextPrimeInt( 541 ); NextPrimeInt( -1 );
    547
    2

PrevPrimeInt
~~~~~~~~~~~~~~

- ``PrevPrimeInt(n)`` (función)

PrevPrimeInt_ devuelve el número primo más grande, que es estrictamente menor que el entero :math:`n`. Tenga en cuenta que PrevPrimeInt_ utiliza una prueba de primalidad probable (consulte IsPrimeInt_).

.. code-block:: gap
    
    gap> PrevPrimeInt( 541 ); PrevPrimeInt( 1 );
    523
    -2

.. _FactorsInt:

FactorsInt
~~~~~~~~~~~~~

- ``FactorsInt(n)`` (función)
- ``FactorsInt(n: RhoTrials := trials)`` (función)

FactorsInt_ devuelve una lista de factores de un entero :math:`n` dado tal que ``Product(FactorsInt(n)) = n``. Si :math:`| n | \leq 1` se devuelve la lista ``[n]``. De lo contrario, el resultado contiene números primos probables, ordenados por valor absoluto. Todas las entradas serán positivas excepto la primera en caso de una :math:`n` negativa.

Consulte PrimeDivisors_ para conocer una función que devuelve un conjunto de números primos (probables) que dividen :math:`n`.

Tenga en cuenta que FactorsInt_ utiliza una prueba de primalidad probable (consulte IsPrimeInt_). Por lo tanto, FactorsInt_ podría devolver una lista que contiene enteros compuestos. En tal caso, recibirá una advertencia sobre el uso de un cebado probable. Puede desactivar estas advertencias mediante ``SetInfoLevel(InfoPrimeInt, 0)``; (también vea ``SetInfoLevel (7.4.3)``).

El tiempo que tarda FactorsInt_ es aproximadamente proporcional a la raíz cuadrada del segundo factor primo más grande de :math:`n`, que es el último que FactorsInt_ tiene que encontrar, ya que el factor más grande es simplemente lo que queda cuando todos los demás han sido eliminados. Por tanto, el tiempo está aproximadamente limitado por la cuarta raíz de :math:`n`. Se garantiza que FactorsInt_ encontrará todos los factores menores que :math:`106` y encontrará la mayoría de los factores menores que :math:`1010`. Si :math:`n` contiene múltiples factores mayores que eso, es posible que FactorsInt_ no pueda factorizar :math:`n` y luego señalará un error.

FactorsInt_ se utiliza en un método para las operaciones generales de ``Factors``.

En la segunda forma, FactorsInt_ llama a ``FactorsRho`` con un límite de ensayos en el número de ensayos que realiza. El valor predeterminado es :math:`8192`. Tenga en cuenta que la ``Rho`` de Pollard es el método más rápido para encontrar factores primos con aproximadamente :math:`5-10` dígitos decimales, pero se vuelve cada vez más inferior a otras técnicas de factorización como por ejemplo el método de curvas elípticas (ECM) cuanto mayores son los factores primos. Por lo tanto, en lugar de realizar una gran cantidad de pruebas de ``Rho``, generalmente es más recomendable instalar el paquete ``FactInt`` y luego simplemente usar la operación ``Factors``. La factorización del octavo número de Fermat por ``Rho`` de Pollard a continuación ya lleva un tiempo.

.. code-block:: gap
    
    gap> FactorsInt( -Factorial(6) );
    [ -2, 2, 2, 2, 3, 3, 5 ]
    gap> Set( FactorsInt( Factorial(13)/11 ) );
    [ 2, 3, 5, 7, 13 ]
    gap> FactorsInt( 2^63 - 1 );
    [ 7, 7, 73, 127, 337, 92737, 649657 ]
    gap> FactorsInt( 10^42 + 1 );
    [ 29, 101, 281, 9901, 226549, 121499449, 4458192223320340849 ]
    gap> FactorsInt(2^256+1:RhoTrials:=100000000);
    [ 1238926361552897, 93461639715357977769163558199606896584051237541638188580280321 ]


.. _PrimeDivisors:

PrimeDivisors
~~~~~~~~~~~~~~~

- ``PrimeDivisors(n)`` (attribute)

PrimeDivisors_ devuelve un número entero distinto de cero n un conjunto de sus divisores primos positivos (probables). En casos raros, el resultado podría contener un número compuesto que pasó ciertas pruebas de primalidad, consulte IsProbablyPrimeInt_ y FactorsInt_ para obtener más detalles.

.. code-block:: gap
    
    gap> PrimeDivisors(-12);
    [ 2, 3 ]
    gap> PrimeDivisors(1);
    [ ]


.. _PartialFactorization:

PartialFactorization
~~~~~~~~~~~~~~~~~~~~~

- ``PartialFactorization(n[, effort])`` (operación)

PartialFactorization_ devuelve una factorización parcial del entero :math:`n`. No se hacen afirmaciones sobre la primordialidad de los factores, excepto los que se mencionan a continuación.

El esfuerzo ``effort``, si se da, especifica la intensidad con la que la función debe intentar determinar los factores de :math:`n`.. El valor predeterminado es ``effort = 5``.

- Si ``effort = 0``, se realiza la división de prueba por los números primos por debajo de :math:`100`. Se garantiza que los factores devueltos por debajo de :math:`104` son primos.

- Si ``effort = 1``, se realiza la división de prueba por los números primos por debajo de :math:`1000`. Se garantiza que los factores devueltos por debajo de :math:`106` son primos.

- Si ``effort = 2``, adicionalmente se realiza una división de prueba por los números de las listas ``Primes2`` y ``ProbablePrimes2``, y se detectan las potencias perfectas. Se garantiza que los factores devueltos por debajo de 106 son primos.

- Si ``effort = 3``, se usa adicionalmente ``FactorsRho`` (Rho de Pollard) con ``RhoTrials = 256``.

- Si ``effort = 4``, como arriba, pero ``RhoTrials = 2048``.

- Si ``effort = 5``, como arriba, pero ``RhoTrials = 8192``. Se garantiza que los factores devueltos por debajo de :math:`1012` son primos, y se garantiza que se encontrarán todos los factores primos por debajo de :math:`106`.

- Si ``effort = 6`` y se carga el paquete ``FactInt``, además de lo anterior se manejan bastantes casos especiales.

- Si ``effort = 7`` y el paquete ``FactInt`` está cargado, lo único que no se intenta para obtener una factorización completa en pseudoprimes Baillie-Pomerance-Selfridge-Wagstaff es la aplicación del MPQS a un compuesto restante con más de :math:`50` dígitos decimales.

Aumentar el valor del effort de argumento en uno generalmente da como resultado un aumento de los requisitos de tiempo de ejecución en un factor de (¡muy aproximadamente!) :math:`3` a :math:`10`. (Consulte también CheapFactorsInt (EDIM: CheapFactorsInt)).

.. code-block:: gap

    gap> List([0..5],i->PartialFactorization(97^35-1,i));
    [ [ 2, 2, 2, 2, 2, 3, 11, 31, 43, 2446338959059521520901826365168917110105972824229555319002965029 ],
      [ 2, 2, 2, 2, 2, 3, 11, 31, 43, 967, 2529823122088440042297648774735177983563570655873376751812787 ],
      [ 2, 2, 2, 2, 2, 3, 11, 31, 43, 967, 2529823122088440042297648774735177983563570655873376751812787 ],
      [ 2, 2, 2, 2, 2, 3, 11, 31, 43, 967, 39761, 262321, 242549173950325921859769421435653153445616962914227 ],
      [ 2, 2, 2, 2, 2, 3, 11, 31, 43, 967, 39761, 262321, 687121, 352993394104278463123335513593170858474150787 ],
      [ 2, 2, 2, 2, 2, 3, 11, 31, 43, 967, 39761, 262321, 687121, 20241187, 504769301, 34549173843451574629911361501 ] ]
    gap>


.. _PrintFactorsInt:

PrintFactorsInt
~~~~~~~~~~~~~~~~~

- ``PrintFactorsInt(n)`` (función)

imprime la factorización prima del entero :math:`n` en forma legible por humanos. Consulte también ``StringPP``.

.. code-block:: gap
    
    gap> PrintFactorsInt( Factorial( 7 ) );
    2^4*3^2*5*7

.. _PrimePowersInt:

PrimePowersInt
~~~~~~~~~~~~~~~

- ``PrimePowersInt(n)`` (función)

devuelve la factorización prima del entero :math:`n` como una lista :math:`[p^{1}, e^{1}, \dots, p^{k}, e^{k}]` con :math:`n = p_{1}^{e_{1}} \cdot p_{2}^{e_{2}} \cdot{\dots}\cdot p_{k}^{e_{k}}`. Para enteros negativos, se toma el valor absoluto. No se permite cero como entrada.

.. code-block:: gap
    
    gap> PrimePowersInt( Factorial( 7 ) );
    [ 2, 4, 3, 2, 5, 1, 7, 1 ]
    gap> PrimePowersInt( Factorial( 15 ) );
    [ 2, 11, 3, 6, 5, 3, 7, 2, 11, 1, 13, 1 ]
    gap> PrimePowersInt( 1 );
    [ ]

.. _DivisorsInt:

DivisorsInt
~~~~~~~~~~~~

- ``DivisorsInt(n)`` (función)

DivisorsInt_ devuelve una lista de todos los divisores del entero :math:`n`. La lista está ordenada, de modo que comienza con :math:`1` y termina con :math:`n`. Definimos que ``DivisorsInt(-n) = DivisorsInt(n)``.

Dado que el conjunto de divisores de :math:`0` es infinito, la llamada ``DivisorsInt(0)`` provoca un error.

DivisorsInt_ puede llamar a FactorsInt_ para obtener los factores primos. ``Sigma`` y ``Tau`` calculan la suma y el número de divisores positivos, respectivamente.

.. code-block:: gap
    
    gap> DivisorsInt( 1 );
    [ 1 ]
    gap> DivisorsInt( 20 );
    [ 1, 2, 4, 5, 10, 20 ]
    gap> DivisorsInt( 541 );
    [ 1, 541 ]

.. _clases-residuales:

Anillos de clases residuales
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

ZmodnZ_ devuelve un anillo de clase de residuo de ``Integers`` módulo un ideal. Estos anillos de clase de residuo son anillos, por lo que se aplican todas las operaciones para anillos.

\mod (for residue class rings)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- ``\mod(r/s, n)`` (operación)

Si ``r``, ``s`` y ``n`` son números enteros, ``r / s`` como fracción reducida es ``p / q``, donde qyn son coprimos, entonces ``r / s mod n`` se define como el producto de ``p`` y la inversa de ``q`` módulo ``n``.

Con la definición anterior, ``4/6 mod 32`` es igual a ``2/3 mod 32`` y, por lo tanto, existe (y es igual a ``22``), a pesar de que ``6`` no tiene inverso módulo ``32``

.. _ZmodnZ:

ZmodnZ
~~~~~~~~

- ``ZmodnZ(n)`` (función)
- ``ZmodpZ(p)`` (función)
- ``ZmodpZNC(p)`` (función)

ZmodnZ_ devuelve un anillo :math:`R` isomorfo al anillo de clase de residuo de los enteros módulo el ideal generado por :math:`n`. El elemento correspondiente a la clase de residuo del entero :math:`i` en este anillo se puede obtener mediante ``i * One(R)``, y un representante de la clase de residuo correspondiente al elemento :math:`x \in R` se puede calcular mediante ``Int(x)``.

- ``ZmodnZ(n)`` es igual a ``Integers mod n``.

- ``ZmodpZ`` hace lo mismo si el argumento :math:`p` es un número entero primo, además, el resultado es un campo.

- ``ZmodpZNC`` omite la comprobación de si :math:`p` es primo.

Cada anillo devuelto por estas funciones contiene la familia completa de sus elementos si :math:`n` no es un primo, y está incrustado en la familia de elementos de campo finito de la característica :math:`n` si :math:`n` es un primo.

.. _ZmodnZObj:

ZmodnZObj (para una familia de clase de residuo y un entero)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


- ``ZmodnZObj(Fam, r)`` (operación)

- ``ZmodnZObj(r, n)`` (operación)

Si el primer argumento es una familia de clases residuales ``Fam``, entonces ZmodnZObj_ devuelve el elemento en ``Fam`` cuya clase lateral está representada por el entero ``r``.

Si los dos argumentos son un entero ``r`` y un entero positivo ``n``, ZmodnZObj_ devuelve el elemento en ``ZmodnZ(n)`` (consulte ZmodnZ_ ) cuya clase lateral está representada por el entero ``r``.

.. code-block:: gap
    
    gap> r:= ZmodnZ(15);
    (Integers mod 15)
    gap> fam:=ElementsFamily(FamilyObj(r));;
    gap> a:= ZmodnZObj(fam,9);
    ZmodnZObj( 9, 15 )
    gap> a+a;
    ZmodnZObj( 3, 15 )
    gap> Int(a+a);
    3

.. _IsZmodnZObj:

IsZmodnZObj
~~~~~~~~~~~~


- ``IsZmodnZObj(obj)`` (Categoría)

- ``IsZmodnZObjNonprime(obj)`` (Categoría)

- ``IsZmodpZObj(obj)`` (Categoría)

- ``IsZmodpZObjSmall(obj)`` (Categoría)

- ``IsZmodpZObjLarge(obj)`` (Categoría)

Los elementos de los anillos :math:`\mathbb{Z} / n\mathbb{Z}` están en la categoría IsZmodnZObj_. Sin es primo, los elementos también están, por supuesto, en la categoría ``IsFFE``; de lo contrario, están en ``IsZmodnZObjNonprime``.

``IsZmodpZObj ``es una abreviatura de IsZmodnZObj_ e ``IsFFE``. Esta categoría es la unión disjunta de ``IsZmodpZObjSmall`` e ``IsZmodpZObjLarge``, el primero que contiene todos los elementos con :math:`n` como máximo ``MAXSIZE_GF_INTERNAL``.

Las razones para distinguir el caso principal del caso no prioritario son

- que los objetos en ``IsZmodnZObjNonprime`` tienen una representación externa (es decir, el residuo en el rango :math:`[0, 1, \dots, n - 1]`),

- que la comparación de elementos puede definirse como una comparación de los residuos, y

- que los elementos pertenecen a una familia de tipo ``IsZmodnZObjNonprimeFamily`` (tenga en cuenta que para el primer :math:`n`, la familia debe ser una ``IsFFEFamily``).

Las razones para distinguir el caso pequeño y grande son que para :math:`n` pequeño los elementos deben ser compatibles con la representación interna de elementos de campo finito, mientras que somos libres de definir la comparación como comparación de residuos para :math:`n` grande.

Tenga en cuenta que no podemos afirmar que cada elemento de campo finito de grado :math:`1` está en IsZmodnZObj_, ya que los elementos de campo finito en representación interna pueden no saber que se encuentran en el campo principal.

Chequear Digitos
-----------------

.. _CheckDigitISBN:

CheckDigitISBN
~~~~~~~~~~~~~~~~~~~~~~~~~~

- ``CheckDigitISBN(n)`` (función)

- ``CheckDigitISBN13(n)`` (función)

- ``CheckDigitPostalMoneyOrder(n)`` (función)

- ``CheckDigitUPC(n)`` (función)

Estas funciones se pueden utilizar para calcular, o verificar, verificar dígitos para algunos elementos cotidianos. En cada caso, lo que se envía como entrada es el número con el dígito de control (en cuyo caso la función devuelve verdadero o falso) o el número sin el dígito de control (en cuyo caso la función devuelve el dígito de control que falta). El número se puede especificar como un entero, como una cadena (por ejemplo, en el caso de ceros a la izquierda) o como una secuencia de argumentos, cada uno de los cuales representa un solo dígito. Los dígitos de control probados son el ``ISBN`` (International Standard Book Number) de :math:`10` dígitos que utiliza ``CheckDigitISBN`` (dado que la aritmética es el módulo :math:`11`, un dígito :math:`11` se representa con una **X**);

- El **ISBN-13** de :math:`13` dígitos más nuevo con ``CheckDigitISBN13``; los números de giros postales estadounidenses de :math:`11` dígitos que utilizan ``CheckDigitPostalMoneyOrder``; y el código de barras UPC de :math:`12` dígitos que se encuentra en los comestibles mediante ``CheckDigitUPC``.

.. code-block:: gap
    
    gap> CheckDigitISBN("052166103");
    Check Digit is 'X'
    'X'
    gap> CheckDigitISBN("052166103X");
    Checksum test satisfied
    true
    gap> CheckDigitISBN(0,5,2,1,6,6,1,0,3,1);
    Checksum test failed
    false
    gap> CheckDigitISBN(0,5,2,1,6,6,1,0,3,'X'); # note single quotes!
    Checksum test satisfied
    true
    gap> CheckDigitISBN13("9781420094527");
    Checksum test satisfied
    true
    gap> CheckDigitUPC("07164183001");
    Check Digit is 1
    1
    gap> CheckDigitPostalMoneyOrder(16786457155);
    Checksum test satisfied
    true

.. _CheckDigitTestFunction:

CheckDigitTestFunction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- ``CheckDigitTestFunction(l, m, f)`` (función)

Esta función crea funciones de prueba de dígitos de control como CheckDigitISBN_ para esquemas de dígitos de control que utilizan los productos internos con un vector fijo módulo un número. El esquema que crea utilizará cadenas de ``l`` dígitos (incluidos los dígitos de control), el control consiste en tomar el producto estándar del vector de dígitos con el vector fijo ``f`` módulo ``m``; el resultado debe ser :math:`0`. La función devuelve una función que luego puede usarse para probar o determinar dígitos de control.

.. code-block:: gap
    
    gap> isbntest:=CheckDigitTestFunction(10,11,[1,2,3,4,5,6,7,8,9,-1]);
    function( arg... ) ... end
    gap> isbntest("038794680");
    Check Digit is 2
    2


Random Sources
-----------------------

GAP proporciona métodos ``Random`` para muchas colecciones de objetos. En un nivel inferior, estos métodos utilizan fuentes aleatorias que proporcionan números enteros aleatorios y opciones aleatorias de listas.

.. _IsRandomSource:

IsRandomSource
~~~~~~~~~~~~~~~~

- ``IsRandomSource(obj)`` (Categoría)

Esta es la categoría de objetos de origen aleatorio que se definen para tener, para un objeto ``rs`` en esta categoría, métodos disponibles para las siguientes operaciones que se explican con más detalle a continuación:

``Random(rs, list)`` dando un elemento aleatorio de una lista, ``Random(rs, low, high)`` dando un número entero aleatorio entre ``low ``y ``high`` (inclusive), ``Init``, State_ y ``Reset``.

Utilice RandomSource_ para construir nuevas fuentes aleatorias.

Una idea detrás de proporcionar varias fuentes independientes (pseudo) aleatorias es hacer algoritmos que utilicen algún tipo de elección aleatoria determinista. Pueden usar su propia fuente aleatoria nueva creada con una semilla fija y hacer exactamente lo mismo en diferentes llamadas.

Los objetos de origen aleatorio pertenecen a la familia ``RandomSourcesFamily``.

.. _random-fuente-aleatoria-y-lista:

Random (para fuente aleatoria y lista)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- ``Random(rs, list)`` (operation)

- ``Random(rs, low, high)`` (operation)

Esta operación devuelve un elemento aleatorio de la lista de la lista, o un número entero en el rango de los números enteros dados (posiblemente grandes) de menor a mayor (``low`` to ``high``), respectivamente.

La elección solo debería depender de las fuentes aleatorias  ``rs`` y no afectará a otras fuentes aleatorias.


.. code-block:: gap
    
    gap> mysource := RandomSource(IsMersenneTwister, 42);;
    gap> Random(mysource, 1, 10^60);
    999331861769949319194941485000557997842686717712198687315183

.. _State:

State
~~~~~~~~~

- ``State(rs)`` (operación)

- ``Reset(rs[, seed])`` (operación)

- ``Init(prers[, seed])`` (operación)

Estas son las operaciones básicas para las que las fuentes aleatorias (consulte IsRandomSource_) deben tener métodos.

State_ debe devolver una estructura de datos que permita recuperar el estado de la fuente aleatoria de modo que se pueda reproducir una secuencia de llamadas aleatorias utilizando esta fuente aleatoria. Si una fuente aleatoria no se puede restablecer (digamos, utiliza datos físicos verdaderamente aleatorios), el estado debería devolver el ``fail``.

``Reset(rs, seed)`` restablece la fuente aleatoria ``rs`` a un estado descrito por ``seed``, si la fuente aleatoria se puede restablecer (de lo contrario, no debería hacer nada). Aquí, la semilla puede ser una salida de State_ y luego debería restablecerse a ese estado. Además, los métodos siempre deben permitir números enteros como semilla. Sin el argumento semilla, se usa la forma predeterminada ``seed = 1``.

``Init`` es el constructor de una fuente aleatoria, obtiene un objeto componente vacío prers que ya tiene el tipo correcto y debe completar los datos reales que se necesitan. Opcionalmente, debería permitir que uno especifique una semilla para el estado inicial, como se explica para Restablecer.

La mayoría de los métodos para ``Random`` en la biblioteca GAP utilizan ``GlobalMersenneTwister`` como fuente aleatoria. Se puede restablecer a un estado conocido como en el siguiente ejemplo.

.. code-block:: gap
    
    gap> seed := Reset(GlobalMersenneTwister);;
    gap> seed = State(GlobalMersenneTwister);
    true
    gap> List([1..10],i->Random(Integers));
    [ -3, 2, -1, -2, -1, -1, 1, -4, 1, 0 ]
    gap> List([1..10],i->Random(Integers));
    [ -1, -1, -1, 1, -1, 1, -2, -1, -2, 0 ]
    gap> Reset(GlobalMersenneTwister, seed);;
    gap> List([1..10],i->Random(Integers));
    [ -3, 2, -1, -2, -1, -1, 1, -4, 1, 0 ]


.. _IsMersenneTwister:

IsMersenneTwister
~~~~~~~~~~~~~~~~~~

- ``IsMersenneTwister(rs)`` (Categoría)

- ``IsGAPRandomSource(rs)`` (Categoría)

- ``IsGlobalRandomSource(rs)`` (Categoría)

- ``GlobalMersenneTwister`` (variable global)

- ``GlobalRandomSource`` (variable global)

Actualmente, la biblioteca GAP proporciona tres tipos de fuentes aleatorias, que se distinguen por las tres categorías enumeradas.

``IsMersenneTwister`` son fuentes aleatorias que utilizan un generador aleatorio rápido de números de :math:`32` bits, llamado Twister de Mersenne. La secuencia pseudoaleatoria tiene un período de :math:`2^{19937}-1` y los números tienen una equidistribución de :math:`623` dimensiones. Para obtener más detalles y el origen del código utilizado en el `kernel <http://www.math.sci.hiroshima-u.ac.jp/~m-mat/MT/emt.html.>`_ de GAP. 

Utilice el tornado de Mersenne si es posible, en particular para generar muchos números enteros aleatorios grandes.

También hay una fuente aleatoria global predefinida ``GlobalMersenneTwister`` que es utilizada por la mayoría de los métodos de biblioteca para ``Random``.

``IsGAPRandomSource`` utiliza el mismo generador de números que ``IsGlobalRandomSource``, pero puede crear varias de estas fuentes aleatorias que generan sus números aleatorios independientemente de todas las demás fuentes aleatorias.

``IsGlobalRandomSource`` da acceso al generador aleatorio global clásico que fue utilizado por GAP en versiones anteriores. No es necesario construir nuevas fuentes aleatorias de este tipo, que utilizarían todas la misma estructura de datos global. Simplemente use la fuente aleatoria existente ``GlobalRandomSource``.

Esto usa el generador aditivo de números aleatorios descrito en [Knu98] (algoritmo A en 3.2.2 con retraso 30).

.. _RandomSource:

RandomSource
~~~~~~~~~~~~~~

- ``RandomSource(cat[, seed])`` (operación)

Esta operación se utiliza para crear nuevas fuentes aleatorias. El primer argumento cat es la categoría que describe el tipo de generador aleatorio, se puede dar una semilla opcional que puede ser un número entero o una estructura de datos específica de tipo para especificar el estado inicial.

.. code-block:: gap
    
    gap> rs1 := RandomSource(IsMersenneTwister);
    <RandomSource in IsMersenneTwister>
    gap> state1 := State(rs1);;
    gap> l1 := List([1..10000], i-> Random(rs1, [1..6]));;
    gap> rs2 := RandomSource(IsMersenneTwister);;
    gap> l2 := List([1..10000], i-> Random(rs2, [1..6]));;
    gap> l1 = l2;
    true
    gap> l1 = List([1..10000], i-> Random(rs1, [1..6]));
    false
    gap> n := Random(rs1, 1, 2^220);
    1077726777923092117987668044202944212469136000816111066409337432400


Bitfields
-------------

Los campos de bits son una característica de bajo nivel destinada a admitir la subdivisión eficiente de enteros inmediatos en campos de bits de varios anchos. Esto suele ser útil para implementar estructuras de datos eficientes en cuanto al espacio y/o la memoria caché. Esta característica debe usarse con cuidado porque (entre otras cosas) tiene diferentes limitaciones en arquitecturas de :math:`32` y :math:`64` bits.

.. _MakeBitfields:

MakeBitfields
~~~~~~~~~~~~~~~~~

- ``MakeBitfields(width.... )`` (function)

Esta función configura la maquinaria para un conjunto de campos de bits de los anchos dados. Todos los valores de campo de bits se tratan como sin signo. El total de los anchos no debe exceder los :math:`60` bits en la arquitectura de 64 bits o los :math:`28` bits en una arquitectura de :math:`32` bits. Por razones de rendimiento, se omiten algunas comprobaciones que uno podría desear hacer. En particular, las funciones de constructor y definidor no verifican si los valores que se les pasan son negativos o demasiado grandes (a menos que GAP esté especialmente compilado para depuración). El comportamiento cuando se pasan tales argumentos no está definido. Puede saber en qué tipo de arquitectura se está ejecutando accediendo a ``GAPInfo.BytesPerVariable``, que es :math:`8` en :math:`64` bits y :math:`4` en :math:`32`. El valor de retorno cuando se dan n anchos es un registro cuyos campos son

- ``widths``

    una copia de los argumentos, por conveniencia,

- ``getters``

    una lista de :math:`n` funciones de un argumento, cada una de las cuales extrae uno de los campos de un entero inmediato

- ``setters``

    una lista de :math:`n` funciones, cada una de las cuales toma dos argumentos: un valor empaquetado y un nuevo valor para uno de sus campos y devuelve un nuevo valor empaquetado. La función :math:`i`-ésima devolvió el nuevo valor empaquetado en el que el campo :math:`i`-ésimo se reemplazó por el nuevo valor. Tenga en cuenta que esto NO modifica el valor empaquetado original.

Pueden estar presentes dos campos adicionales si cualquiera de los anchos de campo es uno. Cada uno es una lista y solo tiene encuadernado en las posiciones correspondientes a los campos de ancho :math:`1`.

- ``booleanGetters``

    si se establece la :math:`i`-ésima posición de esta lista, contiene una función que extrae el :math:`i`-ésimo campo (que tendrá un ancho de uno) y devuelve verdadero si contiene :math:`1` y falso si contiene :math:`0`.

- ``booleanSetters``

    si se establece la :math:`i`-ésima posición de esta lista, contiene una función de dos argumentos. El primer argumento es un valor empaquetado, el segundo es verdadero o falso. Devuelve un nuevo valor empaquetado en el que el :math:`i`-ésimo campo se establece en :math:`1` si el segundo argumento era verdadero y :math:`0` si era falso. El comportamiento de cualquier otro valor no está definido.


.. _BuildBitfields:

BuildBitfields
~~~~~~~~~~~~~~~~~~~~~~

- ``BuildBitfields(widths, vals... )`` (function)

Esta función toma uno o más argumentos. Su primer argumento es una lista de anchos de campo, como se encuentra en la entrada de anchos de un registro devuelto por MakeBitfields_. Los argumentos restantes son valores enteros sin signo, iguales en número a las entradas de la lista de anchos de campo. Devuelve un pequeño entero en el que esas entradas se empaquetan en campos de bits de los anchos dados. La primera entrada ocupa los bits menos significativos. ``DeclareGlobalFunction("BuildBitfields")``;


.. code-block:: gap

    bf := MakeBitfields(1,2,3);
    rec( booleanGetters := [ function( data ) ... end ], booleanSetters := [ function( data, val ) ... end ],
      getters := [ function( data ) ... end, function( data ) ... end, function( data ) ... end ],
      setters := [ function( data, val ) ... end, function( data, val ) ... end, function( data, val ) ... end ],
      widths := [ 1, 2, 3 ] )
    gap> x := BuildBitfields(bf.widths,0,3,5);
    46
    gap> bf.getters[3](x);
    5
    gap> y := bf.setters[1](x,1);
    47
    gap> x;
    46
    gap> bf.booleanGetters[1](x);
    false
    gap> bf.booleanGetters[1](y);
    true
