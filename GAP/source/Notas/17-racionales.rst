Números Racionales
==================

Rational
-----------

Los racionales forman un cuerpo muy importante. Por un lado, es el cuerpo del cociente de los números enteros (Ver :doc:`14-enteros`). Por otro lado, es el cuerpo principal de los cuerpos de característica cero (ver capítulo 60).

El primer comentario sugiere la representación realmente utilizada. Un racional se representa como un par de números enteros, llamados numerador y denominador. El numerador y el denominador se reducen, es decir, su máximo común divisor es :math:`1`. Si el denominador es :math:`1`, el racional es de hecho un número entero y se representa como tal. El numerador tiene el signo de lo racional, por lo que el denominador siempre es positivo.

Debido a que la aritmética de enteros subyacente puede calcular con enteros de tamaño arbitrario, la aritmética racional es siempre exacta, incluso para racionales cuyos numeradores y denominadores tienen miles de dígitos.

.. code-block:: gap
    
    gap> 2/3;
    2/3
    gap> 66/123; # numerator and denominator are made relatively prime
    22/41
    gap> 17/-13; # the numerator carries the sign;
    -17/13
    gap> 121/11; # rationals with denominator 1 (when canceled) are integers
    11

Rationals: Global Variables
---------------------------

.. _IsRationals:

Rationals
~~~~~~~~~

- ``Rationals`` (global variable)

- ``IsRationals(obj)`` (filter)

:ref:`IsRationals` es el cuerpo :math:`\mathbb{Q}` de números enteros racionales, como un conjunto de :doc:`18-ciclotomics`. Las funciones para el cuerpo Racionales se pueden encontrar en los capítulos 58 y 60.

IsRationals_ devuelve ``true`` para un cuerpo primo que consta de números ciclotómicos (por ejemplo, el objeto GAP Rationals) y ``false`` para todos los demás objetos GAP.

.. code-block:: gap
    
    gap> Size( Rationals );
    infinity
    gap> 2/3 in Rationals;
    true

Elementary Operations for Rationals
-----------------------------------

.. _IsRat:

IsRat
~~~~~~

- ``IsRat(obj)`` (Category)


Todo número racional pertenece a la categoría IsRat_, que es una subcategoría de ``IsCyc``.

.. code-block:: gap
    
    gap> IsRat( 2/3 );
    true
    gap> IsRat( 17/-13 );
    true
    gap> IsRat( 11 );
    true
    gap> IsRat( IsRat ); # ‘IsRat’ is a function, not a rational
    false

IsPosRat
~~~~~~~~

- ``IsPosRat(obj)`` (Category)

Todo número racional positivo pertenece a la categoría IsPosRat_.

.. _IsNegRat:

IsNegRat
~~~~~~~~

- ``IsNegRat(obj)`` (Category)

Todo número racional negativo se encuentra en la categoría IsNegRat_.

NumeratorRat
~~~~~~~~~~~~

- ``NumeratorRat(rat)`` (function)

NumeratorRat_ devuelve el numerador del racional ``rat``. Debido a que el numerador tiene el signo del racional, puede ser cualquier número entero. Los enteros son racionales con denominador :math:`1`, por lo que NumeratorRat es la función de identidad para los enteros.

.. code-block:: gap
    
    gap> NumeratorRat( 2/3 );
    2
    gap> # numerator and denominator are made relatively prime:
    gap> NumeratorRat( 66/123 );
    22
    gap> NumeratorRat( 17/-13 ); # numerator holds the sign of the rational
    -17
    gap> NumeratorRat( 11 ); # integers are rationals with denominator 1
    11

.. _DenominatorRat:

DenominatorRat
~~~~~~~~~~~~~~

- ``DenominatorRat(rat)`` (function)

DenominatorRat_ devuelve el denominador de racional ``rat``. Debido a que el numerador tiene el signo del racional, el denominador es siempre un número entero positivo. Los enteros son racionales con el denominador 1, por lo que DenominatorRat_ devuelve :math:`1` para los enteros.

.. code-block:: gap
    
    gap> DenominatorRat( 2/3 );
    3
    gap> # numerator and denominator are made relatively prime:
    gap> DenominatorRat( 66/123 );
    41
    gap> # the denominator holds the sign of the rational:
    gap> DenominatorRat( 17/-13 );
    13
    gap> DenominatorRat( 11 ); # integers are rationals with denominator 1
    1

.. _Rat:

Rat
~~~

- ``Rat(elm)`` (attribute)

Rat devuelve un número racional rata cuyo significado depende del tipo de olmo. Si ``elm`` es una cadena que consta de dígitos ``"0"``, ``"1"``,. . ., ``"99"`` y ``"-""`` (en la primera posición), "``/``" y el punto decimal "``.``" Entonces rata es el racional descrito por esta cadena. Si ``elm`` es un número racional, Rat devuelve ``elm``. La operación String (27.7.6) se puede utilizar para calcular una cadena para números racionales, de hecho para todas las ciclotómicas.

.. code-block:: gap
    
    gap> Rat( "1/2" ); Rat( "35/14" ); Rat( "35/-27" ); Rat( "3.14159" );
    1/2
    5/2
    -35/27
    314159/100000

.. _Random-rationals:

Random (para racionales)
~~~~~~~~~~~~~~~~~~~~~~~~~

- ``Random(Rationals)`` (operation)

:ref:`Random-rationals` devuelve racionales pseudoaleatorios que son el cociente de dos números enteros aleatorios. Consulte la descripción de :ref:`Random-integer` para obtener más detalles. (Ver también Aleatorio (30.7.1).)