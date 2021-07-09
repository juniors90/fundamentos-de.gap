Matrices
===============

Las matrices están representadas en GAP por listas de vectores de fila (para cambios futuros a esta política, ver Capítulo 26). Todos los vectores deben tener la misma longitud y sus elementos deben estar en un anillo común.

Sin embargo, dado que verificar la rectangularidad puede ser funciones costosas y los métodos de operación para matrices, a menudo no darán un mensaje de error para listas de listas no rectangulares, en tales casos el resultado no está definido.

Debido a que las matrices son solo un caso especial de listas, todas las operaciones y funciones de las listas también son aplicables a las matrices. Esto incluye especialmente acceder a elementos de una matriz, cambiar elementos de una matriz y comparar matrices.

Tenga en cuenta que, dado que una matriz es una lista de listas, el comportamiento de ``ShallowCopy (12.7.1)`` para matrices es solo un caso especial de ``ShallowCopy (12.7.1)`` para listas; llamada con una matriz inmutable ``mat``, ``ShallowCopy (12.7.1)`` devuelve una matriz mutable cuyas filas son idénticas a las filas de ``mat``. En particular, las filas siguen siendo inmutables. Para obtener una matriz cuyas filas sean mutables, se puede usar ``List( mat, ShallowCopy )``.


InfoMatrix (Info Class)
-------------------------

.. _InfoMatrix:

InfoMatrix
~~~~~~~~~~~

- ``InfoMatrix`` (clase de información)

La clase de información para operaciones matriciales es InfoMatrix_.

Categorías de Matrices
-------------------------

IsMatrix
~~~~~~~~~~~

- ``IsMatrix(obj)`` (Categoría)

Por convención, la matriz es una lista de listas de igual longitud cuyas entradas se encuentran en un anillo común.

.. important::
    
    Por razones técnicas expuestas en la parte superior del Capítulo :doc:`24-matrices`, el filtro IsMatrix_ es sinónimo de una tabla de elementos de anillo (consultar ``IsTable`` e ``IsRingElement``). Esto significa que IsMatrix_ devuelve verdadero para tablas como ``[[1,2], [3]]``. Si es necesario, ``IsRectangularTable`` se puede utilizar para probar manualmente si un objeto es una lista de listas homogéneas de igual longitud.

Tenga en cuenta que las matrices pueden tener diferentes multiplicaciones, además del producto matricial habitual, existe, por ejemplo, el producto de Lie. Entonces, hay categorías como IsOrdinaryMatrix_ e IsLieMatrix_ que describen la multiplicación de matrices. Se puede formar el producto de dos matrices solo si admiten la misma multiplicación.

.. code-block:: gap
    
    gap> mat:=[[1,2,3],[4,5,6],[7,8,9]];
    [ [ 1, 2, 3 ], [ 4, 5, 6 ], [ 7, 8, 9 ] ]
    gap> IsMatrix(mat);
    true
    gap> IsRectangularTable(mat);
    true
    gap> mat:=[[1,2],[3]];
    [ [ 1, 2 ], [ 3 ] ]
    gap> IsMatrix(mat);
    true
    gap> IsRectangularTable(mat);
    false

Tenga en cuenta que la lista vacía ``[]`` y las estructuras "vacías" más complejas como ``[[]]`` no son matrices, aunque métodos especiales permiten que se usen en lugar de matrices en algunas situaciones. Consulte ``EmptyMatrix (24.5.3)`` a continuación.

.. code-block:: gap
    
    gap> [[0]]*[[]];
    [ [ ] ]
    gap> IsMatrix([[]]);
    false


.. _IsOrdinaryMatrix:

IsOrdinaryMatrix
~~~~~~~~~~~~~~~~~~~

- ``IsOrdinaryMatrix(obj)`` (Categoría)

Una matriz ordinaria es una matriz cuya multiplicación es la multiplicación de matrices ordinarias.

Cada matriz en representación interna está en la categoría IsOrdinaryMatrix_, y las operaciones aritméticas con objetos en IsOrdinaryMatrix_ producen nuevamente matrices en IsOrdinaryMatrix_.

Tenga en cuenta que queremos que las matrices de Lie sean matrices que se comporten de la misma manera que las matrices ordinarias, excepto que tienen una multiplicación diferente. Por lo tanto, debemos distinguir las diferentes multiplicaciones de matrices, para poder describir la aplicabilidad de la multiplicación, y también para formar una matriz del tipo apropiado como la suma, diferencia, etc. de dos matrices que tienen la misma multiplicación.

.. _IsLieMatrix:

IsLieMatrix
~~~~~~~~~~~~

- ``IsLieMatrix(mat)`` (Categoría)

Una *matriz de Lie* es una matriz cuya multiplicación viene dada por el corchete de Lie. (Tenga en cuenta que una matriz con multiplicación de matrices ordinaria está en la categoría IsOrdinaryMatrix_.)

Cada matriz creada por ``LieObject`` está en la categoría IsLieMatrix_, y las operaciones aritméticas con objetos en IsLieMatrix_ producen nuevamente matrices en IsLieMatrix_.



Operadores para Matrices
---------------------------

Las reglas para las operaciones aritméticas que involucran matrices son de hecho casos especiales de las de la aritmética de listas, dadas en la Sección 21.11 y las siguientes secciones, aquí reiteramos esa definición, en el lenguaje de vectores y matrices.

Tenga en cuenta que el comportamiento aditivo esbozado a continuación se define solo para listas en la categoría ``IsGeneralizedRowVector``, y el comportamiento multiplicativo se define solo para listas en la categoría ``IsMultiplicativeGeneralizedRowVector``.

.. code-block:: gap
    
    mat1 + mat2

devuelve la suma de las dos matrices ``mat1`` y ``mat2``. Probablemente la situación más habitual es que ``mat1`` y ``mat2`` tienen las mismas dimensiones y están definidas en un cuerpo común; en este caso, la suma es una nueva matriz sobre el mismo cuerpo donde cada entrada es la suma de las entradas correspondientes de las matrices.

En situaciones más generales, la suma de dos matrices no necesita ser una matriz, por ejemplo, al agregar una matriz entera ``mat1`` y una matriz ``mat2`` sobre un cuerpo finito, se obtiene la tabla de sumas puntuales, que será una mezcla de elementos de cuerpo finito y números enteros si ``mat1`` tiene dimensiones más grandes que ``mat2``.

.. code-block:: gap
    
    scalar + mat
    mat + scalar

devuelve la suma del escalar escalar y la matriz ``mat``. Probablemente la situación más habitual es que las entradas de ``mat`` se encuentran en un cuerpo común con escalar; en este caso, la suma es una nueva matriz sobre el mismo cuerpo donde cada entrada es la suma del escalar y la entrada correspondiente de la matriz.

Las situaciones más generales son, por ejemplo, la suma de un escalar entero y una matriz sobre un cuerpo finito, o la suma de un elemento de cuerpo finito y una matriz entera.

.. code-block:: gap
    
    mat1 - mat2
    scalar - mat
    mat - scalar

Restar una matriz o un escalar se define como sumar su inverso aditivo, por lo que los enunciados para la suma son válidos del mismo modo.

.. code-block:: gap
    
    scalar * mat
    mat * scalar

devuelve el producto del escalar escalar y la matriz mat. Probablemente la situación más habitual es que los elementos de mat se encuentran en un cuerpo común con escalar; en este caso, el producto es una nueva matriz sobre el mismo cuerpo donde cada entrada es el producto del escalar y la entrada correspondiente de la matriz.

Las situaciones más generales son, por ejemplo, el producto de un escalar entero y una matriz sobre un cuerpo finito, o el producto de un elemento de cuerpo finito y una matriz entera.

.. code-block:: gap

    vec * mat

devuelve el producto del vector de fila ``vec`` y la matriz mat. Probablemente la situación más habitual es que ``vec`` y mat tienen las mismas longitudes y se definen sobre un cuerpo común, y que todas las filas de mat tienen la misma longitud :math:`m`, digamos; en este caso, el producto es un nuevo vector de fila de longitud m sobre el mismo cuerpo que es la suma de los múltiplos escalares de las filas de mat con las correspondientes entradas de ``vec``.

Las situaciones más generales son, por ejemplo, el producto de un vector entero y una matriz sobre un cuerpo finito, o el producto de un vector sobre un cuerpo finito y una matriz entera.

.. code-block:: gap
    
    mat * vec

devuelve el producto de la matriz ``mat`` y el vector de fila ``vec``. (Este es el producto estándar de una matriz con un vector columna.) Probablemente la situación más habitual es que la longitud de vec y de todas las filas de ``mat`` son iguales, y que los elementos de ``mat`` y ``vec`` se encuentran en un cuerpo común; en este caso, el producto es un nuevo vector de fila de la misma longitud que ``mat`` y sobre el mismo cuerpo que es la suma de los múltiplos escalares de las columnas de ``mat`` con las correspondientes entradas de ``vec``.

Las situaciones más generales son, por ejemplo, el producto de una matriz entera y un vector sobre un cuerpo finito, o el producto de una matriz sobre un cuerpo finito y un vector entero.

.. code-block:: gap
    
    mat1 * mat2

Esta forma evalúa el producto (Cauchy) de las dos matrices ``mat1`` y ``mat2.`` Probablemente, la situación más habitual es que el número de columnas de ``mat1`` es igual al número de filas de ``mat2``, y que los elementos de ``mat`` y ``vec`` se encuentran en un cuerpo común; si ``mat1`` es una matriz con :math:`m` filas y :math:`m` columnas, digamos, y ``mat2`` es una matriz con :math:`n` filas y :math:`o` columnas, el resultado es una nueva matriz con :math:`m` filas y :math:`o` columnas. El elemento de la fila :math:`i` en la posición :math:`j` del producto es la suma de ``mat1[i][l] ∗ mat2[l][j]``, donde ``l`` va de :math:`1` a :math:`n`.

.. code-block:: gap
    
    Inverse( mat )

devuelve el inverso de la matriz ``mat``, que debe ser una matriz cuadrada invertible. Si ``mat`` no es invertible, se devuelve el :ref:`fail`.

.. code-block:: gap
    
    mat1 / mat2
    scalar / mat
    mat / scalar
    vec / mat

En general, ``left / right`` se define como ``left * right ^ -1``. Por tanto, en las formas anteriores, el operando derecho siempre debe ser invertible.

.. code-block:: gap
    
    mat ^ int
    mat1 ^ mat2
    vec ^ mat

Elevar una matriz cuadrada ``mat`` por un entero ``int`` da como resultado la potencia ``int`` de mat; si ``int`` es negativo, ``mat`` debe ser invertible, si ``int`` es :math:`0`, el resultado es la matriz de identidad ``One(mat)``, incluso si ``mat`` no es invertible.

Al alimentar una matriz cuadrada ``mat1`` mediante una matriz cuadrada invertible ``mat2`` de las mismas dimensiones se obtiene el conjugado de ``mat1`` por ``mat2``, es decir, la matriz

.. code-block:: gap
    
    mat2^-1 * mat1 * mat2.

Alimentar un vector de fila vec mediante una matriz ``mat`` es en todos los aspectos equivalente a ``vec * mat``. Estas operaciones reflejan el hecho de que las matrices actúan de forma natural en los vectores de fila mediante la multiplicación desde la derecha, y que el operador de potencia es el estándar de GAP para acciones de grupo.

.. code-block:: gap
    
    Comm( mat1 , mat2 )

devuelve el conmutador de las matrices cuadradas invertibles ``mat1`` y ``mat2`` de las mismas dimensiones y sobre un cuerpo común, que es la matriz ``mat1^-1 * mat2^-1 * mat1 * mat2``.

Los siguientes casos siguen siendo casos especiales de la aritmética de lista general.

.. code-block:: gap
    
    scalar + matlist
    matlist + scalar
    scalar - matlist
    matlist - scalar
    scalar * matlist
    matlist * scalar
    matlist / scalar

Un escalar escalar también se puede sumar, restar, multiplicar o dividir en una lista de matrices. El resultado es una nueva lista de matrices donde cada matriz es el resultado de realizar la operación con la matriz correspondiente en ``matlist``.

.. code-block:: gap
    
    mat * matlist
    matlist * mat

Una matriz ``mat`` también se puede multiplicar por una lista de matrices ``matlist``. El resultado es una nueva lista de matrices, donde cada entrada es el producto de ``mat`` y la entrada correspondiente en ``matlist``.

.. code-block:: gap
    
    matlist / mat

Dividir una lista matlist de matrices por una matriz de matriz invertible ``mat`` se evalúa como ``matlist * mat^-1``.

.. code-block:: gap
    
    vec * matlist

devuelve el producto del vector ``vec`` y la lista de matrices ``mat``. Las longitudes :math:`l` de ``vec`` y ``matlist`` deben ser iguales. Todas las matrices en ``matlist`` deben tener las mismas dimensiones. Los elementos de ``vec`` y los elementos de las matrices en matlist deben estar en un anillo común. El producto es la suma sobre ``vec[i] * matlist[i]`` con ``i`` corriendo de :math:`1` a :math:`l`.

Para conocer la mutabilidad de los resultados de las operaciones aritméticas, consultar Sección 6 de :doc:`12-objetos-y-elementos`.


Propiedades y Atributos de las Matrices
----------------------------------------


.. _DimensionsMat:

DimensionsMat
~~~~~~~~~~~~~~

- ``DimensionsMat(mat)`` (atributo)

es una lista de longitud :math:`2`, el primero es el número de filas, el segundo es el número de columnas de la matriz ``math``. Si ``mat`` tiene un formato incorrecto, es decir, no es un ``IsRectangularTable``, retorna :ref:`fail`.


.. code-block:: gap
    
    gap> DimensionsMat([[1,2,3],[4,5,6]]);
    [ 2, 3 ]
    gap> DimensionsMat([[1,2,3],[4,5]]);
    fail

.. _DefaultFieldOfMatrix:

DefaultFieldOfMatrix
~~~~~~~~~~~~~~~~~~~~~~


- ``DefaultFieldOfMatrix(mat)`` (atributo)

Para una matriz ``mat``, DefaultFieldOfMatrix_ devuelve un cuerpo (no necesariamente el más pequeño) que contiene todas las entradas de ``mat``, o :ref:`fail`.

Si ``mat`` es una matriz de elementos de cuerpo finito o una matriz de ciclotómica, DefaultFieldOfMatrix_ devuelve el cuerpo predeterminado generado por las entradas de la matriz.

.. code-block:: gap
    
    gap> DefaultFieldOfMatrix([[Z(4),Z(8)]]);
    GF(2^6)


.. _TraceMat:

TraceMat
~~~~~~~~~~~~~

- ``TraceMat(mat)`` (operación)

- ``Trace(mat)`` (atributo)

La traza de una matriz cuadrada es la suma de sus entradas diagonales.

.. code-block:: gap
    
    gap> TraceMat([[1,2,3],[4,5,6],[7,8,9]]);
    15


.. _DeterminantMat:

DeterminantMat
~~~~~~~~~~~~~~~~

- ``DeterminantMat(mat)`` (atributo)

- ``Determinant(mat)`` (operación)

devuelve el determinante de la matriz cuadrada ``mat``.

Estos métodos asumen implícitamente que mat se define sobre un dominio integral cuyo cuerpo de cociente se implementa en GAP. Para matrices definidas sobre un anillo conmutativo arbitrario con uno, consulte DeterminantMatDivFree_.


.. _DeterminantMatDestructive:

DeterminantMatDestructive
~~~~~~~~~~~~~~~~~~~~~~~~~~~

- ``DeterminantMatDestructive(mat)`` (operación)

Hace lo mismo que DeterminantMat_, con la diferencia de que puede destruir su argumento.

La matriz ``mat`` debe ser mutable.

.. code-block:: gap
    
    gap> DeterminantMat([[1,2],[2,1]]);
    -3
    gap> mm:= [[1,2],[2,1]];;
    gap> DeterminantMatDestructive( mm );
    -3
    gap> mm;
    [ [ 1, 2 ], [ 0, -3 ] ]


.. _DeterminantMatDivFree:

DeterminantMatDivFree
~~~~~~~~~~~~~~~~~~~~~~~

- ``DeterminantMatDivFree(mat)`` (operación)

devuelve el determinante de una matriz cuadrada ``mat`` sobre un anillo conmutativo arbitrario con uno utilizando el método libre de división de Mahajan y Vinay.


.. _IsMonomialMatrix:

IsMonomialMatrix
~~~~~~~~~~~~~~~~~~

- ``IsMonomialMatrix(mat)`` (propiedad)

Una matriz es monomial si y solo si tiene exactamente una entrada distinta de cero en cada fila y cada columna.

.. code-block:: gap
    
    gap> IsMonomialMatrix([[0,1],[1,0]]);
    true


.. _IsDiagonalMat:

IsDiagonalMat
~~~~~~~~~~~~~~~~~~

- ``IsDiagonalMat(mat)`` (operación)

devuelve ``true`` si ``mat`` solo tiene cero entradas fuera de la diagonal principal, ``false`` en caso contrario.


.. _IsUpperTriangularMat:

IsUpperTriangularMat
~~~~~~~~~~~~~~~~~~~~~

- ``IsUpperTriangularMat(mat)`` (operación)

devuelve ``true`` si ``math`` solo tiene cero entradas debajo de la diagonal principal, ``false`` en caso contrario.


.. _IsLowerTriangularMat:

IsLowerTriangularMat
~~~~~~~~~~~~~~~~~~~~~

- ``IsLowerTriangularMat(mat)`` (operación)

devuelve ``true`` si ``mat`` solo tiene cero entradas debajo de la diagonal principal, ``false`` en caso contrario.


Constructiones de Matrices
----------------------------------------

.. _IdentityMat:

IdentityMat
~~~~~~~~~~~~

- ``IdentityMat(m[, R])`` (función)

devuelve una matriz de identidad (mutable) :math:`m \times m` sobre el anillo dado por :math:`R`. Aquí, :math:`R` puede ser un anillo o un elemento de un anillo. De forma predeterminada, se crea una matriz de enteros.

.. code-block:: gap
    
    gap> IdentityMat(3);
    [ [ 1, 0, 0 ], [ 0, 1, 0 ], [ 0, 0, 1 ] ]
    gap> IdentityMat(2,Integers mod 15);
    [ [ ZmodnZObj( 1, 15 ), ZmodnZObj( 0, 15 ) ],
    [ ZmodnZObj( 0, 15 ), ZmodnZObj( 1, 15 ) ] ]
    gap> IdentityMat(2,Z(3));
    [ [ Z(3)^0, 0*Z(3) ], [ 0*Z(3), Z(3)^0 ] ]


.. _NullMat:

NullMat
~~~~~~~~

- ``NullMat(m, n[, R])`` (función)

devuelve una matriz nula (mutable) :math:`m \times n` sobre el anillo dado por :math:`R`. Aquí, :math:`R` puede ser un anillo o un elemento de un anillo. De forma predeterminada, se crea una matriz de enteros.

.. code-block:: gap
    
    gap> NullMat(3,2);
    [ [ 0, 0 ], [ 0, 0 ], [ 0, 0 ] ]
    gap> NullMat(2,2,Integers mod 15);
    [ [ ZmodnZObj( 0, 15 ), ZmodnZObj( 0, 15 ) ],
    [ ZmodnZObj( 0, 15 ), ZmodnZObj( 0, 15 ) ] ]
    gap> NullMat(3,2,Z(3));
    [ [ 0*Z(3), 0*Z(3) ], [ 0*Z(3), 0*Z(3) ], [ 0*Z(3), 0*Z(3) ] ]


.. _EmptyMatrix:

EmptyMatrix
~~~~~~~~~~~~

- ``EmptyMatrix(char)`` (función)

es una matriz vacía (ordinaria) en característica ``char`` que se puede agregar o multiplicar con listas vacías (que representan vectores de fila de dimensión cero). También actúa (a través de la operación ``\^`` ) sobre listas vacías.


.. code-block:: gap
    
    gap> EmptyMatrix(5);
    EmptyMatrix( 5 )
    gap> AsList(last);
    [ ]

.. _DiagonalMat:

DiagonalMat
~~~~~~~~~~~~

- ``DiagonalMat(vector)`` (función)

devuelve una matriz diagonal ``mat`` con las entradas diagonales dadas por el ``vector``.

.. code-block:: gap
    
    gap> DiagonalMat([1,2,3]);
    [ [ 1, 0, 0 ], [ 0, 2, 0 ], [ 0, 0, 3 ] ]


.. _PermutationMat:

PermutationMat
~~~~~~~~~~~~~~~~~

- ``PermutationMat(perm, dim[, F])`` (función)

devuelve una matriz en dimensión tenue sobre el cuerpo dado por :math:`\mathbb{F}` (es decir, el cuerpo más pequeño que contiene el elemento :math:`\mathbb{F}` o :math:`\mathbb{F}` en sí mismo si es un cuerpo) que representa la permutación permanente que actúa permutando los vectores base cuando permuta puntos.

.. code-block:: gap
    
    gap> PermutationMat((1,2,3),4,1);
    [ [ 0, 1, 0, 0 ], [ 0, 0, 1, 0 ], [ 1, 0, 0, 0 ], [ 0, 0, 0, 1 ] ]


.. _TransposedMatImmutable:

TransposedMatImmutable
~~~~~~~~~~~~~~~~~~~~~~~


- ``TransposedMatImmutable(mat)`` (atributo)

- ``TransposedMatAttr(mat)`` (atributo)

- ``TransposedMat(mat)`` (atributo)

- ``TransposedMatMutable(mat)`` (operación)

- ``TransposedMatOp(mat)`` (operación)

Todas estas funciones devuelven la transposición de la matriz ``mat``, es decir, una matriz ``trans`` tal que se cumple ``trans[i][k] = mat[k][i]``. Se diferencian solo w.r.t. la mutabilidad del resultado.

``TransposedMat`` es un atributo y, por lo tanto, devuelve un resultado inmutable. Se garantiza que ``TransposedMatMutable`` devolverá una nueva matriz mutable. ``TransposedMatImmutable`` y ``TransposedMatAttr`` son sinónimos de ``TransposedMat``, y ``TransposedMatOp`` es un sinónimo de ``TransposedMatMutable``, en analogía con operaciones como ``Zero``.

.. _TransposedMatDestructive:

TransposedMatDestructive
~~~~~~~~~~~~~~~~~~~~~~~~~~~

- ``TransposedMatDestructive(mat)`` (operación)

Si ``mat`` es una matriz mutable, entonces la transpuesta se calcula intercambiando las entradas en ``mat``. De esta manera se cambia ``mat``. En todos los demás casos, la transpuesta se calcula mediante ``TransposedMat``.


.. code-block:: gap
    
    gap> TransposedMat([[1,2,3],[4,5,6],[7,8,9]]);
    [ [ 1, 4, 7 ], [ 2, 5, 8 ], [ 3, 6, 9 ] ]
    gap> mm:= [[1,2,3],[4,5,6],[7,8,9]];;
    gap> TransposedMatDestructive( mm );
    [ [ 1, 4, 7 ], [ 2, 5, 8 ], [ 3, 6, 9 ] ]
    gap> mm;
    [ [ 1, 4, 7 ], [ 2, 5, 8 ], [ 3, 6, 9 ] ]


.. _KroneckerProduct:

KroneckerProduct
~~~~~~~~~~~~~~~~~

- ``KroneckerProduct(mat1, mat2)`` (operación)

El producto de Kronecker de dos matrices es la matriz obtenida al reemplazar cada entrada ``a`` de ``mat1`` por el producto ``a*mat2`` en una matriz.

.. code-block:: gap
    
    gap> KroneckerProduct([[1,2]],[[5,7],[9,2]]);
    [ [ 5, 7, 10, 14 ], [ 9, 2, 18, 4 ] ]


.. _ReflectionMat:

ReflectionMat
~~~~~~~~~~~~~~

- ``ReflectionMat(coeffs[, conj][, root])`` (función)

Sea ``coeff`` un vector de fila. ReflectionMat_ devuelve la matriz de la reflexión en este vector.

Más precisamente, si ``coeffs`` es la lista de coeficientes de un vector :math:`\nu` w.r.t. una base :math:`\mathcal{B}`, digamos, entonces la matriz devuelta describe la reflexión en :math:`\nu` w.r.t. :math:`\mathcal{B}` como un mapa en un espacio de fila, con acción desde la derecha.

La raíz del argumento opcional es una raíz de unidad que determina el orden de la reflexión. El valor predeterminado es un reflejo del orden :math:`2`. Para las nimiedades, se debe elegir una tercera raíz de la unidad, etc. (ver :ref:`E`).

``conj`` es una función de un argumento que conjuga un elemento de anillo. El valor predeterminado es ``ComplexConjugate (18.5.2)``.

La matriz de la reflexión en :math:`\nu` se define como

.. math::
    
    M = I_{n} + \overline{\nu^{tr}} \cdot (w - 1) / (\nu\overline{\nu^{tr}}) \cdot \nu

donde :math:`w` es igual a la raíz, :math:`n` es la longitud de la lista de coeficientes y denota la conjugación.

Entonces :math:`\nu` se asigna a :math:`w\nu`, con :math:`-\nu` predeterminado, y cualquier vector :math:`x` con la propiedad :math:`x\overline{\nu^{tr}} = 0` está fijado por la reflexión.


.. _PrintArray:

PrintArray
~~~~~~~~~~~~~

- ``PrintArray(array)`` (función)

imprime mejor la matriz ``array``.


Random Matrices
-------------------------

.. _RandomMat:

RandomMat
~~~~~~~~~~~~~

- ``RandomMat([rs, ]m, n[, R])`` (función)

RandomMat_ devuelve una nueva matriz aleatoria mutable con ``m`` filas y ``n`` columnas con elementos tomados del anillo :math:`R`, que por defecto es :doc:`14-enteros`. Opcionalmente, se puede suministrar una fuente aleatoria ``rs``.

.. code-block:: gap
    
    gap> RandomMat(2,3,GF(3));
    [ [ Z(3), Z(3), 0*Z(3) ], [ Z(3), Z(3)^0, Z(3) ] ]


.. _RandomInvertibleMat:

RandomInvertibleMat
~~~~~~~~~~~~~~~~~~~~

- ``RandomInvertibleMat([rs, ]m[, R])`` (función)

RandomInvertibleMat_ devuelve una nueva matriz aleatoria invertible mutable con ``m`` filas y columnas con elementos tomados del anillo :math:`R`, que por defecto es :doc:`14-enteros`. Opcionalmente, se puede suministrar una fuente aleatoria ``rs``.

.. code-block:: gap
    
    gap> m := RandomInvertibleMat(4);
    [ [ -4, 1, 0, -1 ], [ -1, -1, 1, -1 ], [ 1, -2, -1, -2 ],
    [ 0, -1, 2, -2 ] ]
    gap> m^-1;
    [ [ -1/8, -11/24, 1/24, 1/4 ], [ 1/4, -13/12, -1/12, 1/2 ],
    [ -1/8, 5/24, -7/24, 1/4 ], [ -1/4, 3/4, -1/4, -1/2 ] ]


.. _RandomUnimodularMat:

RandomUnimodularMat
~~~~~~~~~~~~~~~~~~~~~~~~~~

- ``RandomUnimodularMat([rs, ]m)`` (función)

devuelve una nueva matriz aleatoria :math:`m \times m` mutable con entradas enteras que es invertible sobre los enteros. Opcionalmente, se puede suministrar una fuente aleatoria ``rs``.

.. code-block:: gap
    
    gap> m := RandomUnimodularMat(3);
    [ [ -5, 1, 0 ], [ 12, -2, -1 ], [ -14, 3, 0 ] ]
    gap> m^-1;
    [ [ -3, 0, 1 ], [ -14, 0, 5 ], [ -8, -1, 2 ] ]




Matrices que representan ecuaciones lineales y el algoritmo gaussiano
----------------------------------------------------------------------

.. _RankMat:

RankMat
~~~~~~~~~

- ``RankMat(mat)`` (atributo)

Si ``mat`` es una matriz cuyas filas abarcan un módulo libre sobre el anillo generado por las entradas de la matriz y sus inversas, RankMat_ devuelve la dimensión de este módulo libre. De lo contrario, se devuelve el error.

Tenga en cuenta que RankMat_ puede realizar una eliminación gaussiana. Para matrices racionales grandes, esto puede llevar mucho tiempo, porque las entradas pueden volverse muy grandes.

.. code-block:: gap
    
    gap> mat:=[[1,2,3],[4,5,6],[7,8,9]];;
    gap> RankMat(mat);
    2

.. _TriangulizedMat:

TriangulizedMat
~~~~~~~~~~~~~~~~

- ``TriangulizedMat(mat)`` (operación)

- ``RREF(mat)`` (operación)

Calcula una forma triangular superior de la matriz matricial mediante el algoritmo gaussiano. Devuelve una matriz mutable en forma triangular superior. Esto a veces también se denomina "forma normal de Hermite" o "forma escalonada de fila reducida". ``RREF`` es sinónimo de TriangulizedMat_.

.. _TriangulizeMat:

TriangulizeMat
~~~~~~~~~~~~~~~

- ``TriangulizeMat(mat)`` (operación)

Aplica el algoritmo gaussiano a la matriz mutable ``mat`` y cambia a ``mat`` modo que esté en forma normal triangular superior (a veces llamada "forma normal de Hermite" o "forma escalonada de fila reducida").

.. code-block:: gap
    
    gap> m:=TransposedMatMutable(mat);
    [ [ 1, 4, 7 ], [ 2, 5, 8 ], [ 3, 6, 9 ] ]
    gap> TriangulizeMat(m);m;
    [ [ 1, 0, -1 ], [ 0, 1, 2 ], [ 0, 0, 0 ] ]
    gap> m:=TransposedMatMutable(mat);
    [ [ 1, 4, 7 ], [ 2, 5, 8 ], [ 3, 6, 9 ] ]
    gap> TriangulizedMat(m);m;
    [ [ 1, 0, -1 ], [ 0, 1, 2 ], [ 0, 0, 0 ] ]
    [ [ 1, 4, 7 ], [ 2, 5, 8 ], [ 3, 6, 9 ] ]


.. _NullspaceMat:

NullspaceMat
~~~~~~~~~~~~~~

- ``NullspaceMat(mat)`` (atributo)

- ``TriangulizedNullspaceMat(mat)`` (atributo)

devuelve una lista de vectores de fila que forman una base del espacio vectorial de soluciones de la ecuación ``vec * mat = 0``. El resultado es una matriz inmutable. No se garantiza que esta base tenga una forma específica.

La variante ``TriangulizedNullspaceMat`` devuelve una base del espacio nulo en forma triangulizada, como suele ser necesario para los algoritmos.

.. _NullspaceMatDestructive:

NullspaceMatDestructive
~~~~~~~~~~~~~~~~~~~~~~~~~~

- ``NullspaceMatDestructive(mat)`` (operación)

- ``TriangulizedNullspaceMatDestructive(mat)`` (operación)

Esta función hace lo mismo que NullspaceMat_. Sin embargo, la última función hace una copia de ``mat`` para evitar tener que cambiarlo. Esta función no hace eso; devuelve el espacio nulo y puede destruir ``mat``; esto ahorra mucha memoria en caso de que ``mat`` sea grande. ``mat`` debe ser una matriz mutable.

La variante ``TriangulizedNullspaceMatDestructive`` devuelve una base del espacio nulo en forma triangulizada. Puede destruir la estera de matriz.

.. code-block:: gap
    
    gap> mat:=[[1,2,3],[4,5,6],[7,8,9]];;
    gap> NullspaceMat(mat);
    [ [ 1, -2, 1 ] ]
    gap> mm:=[[1,2,3],[4,5,6],[7,8,9]];;
    gap> NullspaceMatDestructive( mm );
    [ [ 1, -2, 1 ] ]
    gap> mm;
    [ [ 1, 2, 3 ], [ 0, -3, -6 ], [ 0, 0, 0 ] ]


.. _SolutionMat:

SolutionMat
~~~~~~~~~~~~~

- ``SolutionMat(mat, vec)`` (operación)

devuelve un vector de fila ``x`` que es una solución de la ecuación ``x * mat = vec``. Devuelve :ref:`fail` si no existe tal vector.


.. _SolutionMatDestructive:

SolutionMatDestructive
~~~~~~~~~~~~~~~~~~~~~~~~~

- ``SolutionMatDestructive(mat, vec)`` (operación)

Hace lo mismo que ``SolutionMat(mat, vec)`` excepto que puede destruir la matriz ``mat`` y el vector ``vec``. La matriz ``mat`` debe ser mutable.

.. code-block:: gap
    
    gap> mat:=[[1,2,3],[4,5,6],[7,8,9]];;
    gap> SolutionMat(mat,[3,5,7]);
    [ 5/3, 1/3, 0 ]
    gap> mm:= [[1,2,3],[4,5,6],[7,8,9]];;
    gap> v:= [3,5,7];;
    gap> SolutionMatDestructive( mm, v );
    [ 5/3, 1/3, 0 ]
    gap> mm;
    [ [ 1, 2, 3 ], [ 0, -3, -6 ], [ 0, 0, 0 ] ]
    gap> v;
    [ 0, 0, 0 ]


.. _BaseFixedSpace:

BaseFixedSpace
~~~~~~~~~~~~~~~~~

- ``BaseFixedSpace(mats)`` (función)

BaseFixedSpace_ devuelve una lista de vectores de fila que forman una base del espacio vectorial :math:`V` tal que :math:`vM = v` para todo :math:`v` en :math:`V` y todas las matrices :math:`M` en la lista de ``mats``. (Este es el espacio propio común de todas las matrices en mats para el valor propio :math:`1`).

.. code-block:: gap
    
    gap> BaseFixedSpace([[[1,2],[0,1]]]);
    [ [ 0, 1 ] ]


Eigenvectors and eigenvalues
-----------------------------

GeneralisedEigenvalues
~~~~~~~~~~~~~~~~~~~~~~~


- ``GeneralisedEigenvalues(F, A)`` (operación)

- ``GeneralizedEigenvalues(F, A)`` (operación)

Los valores propios generalizados de la matriz ``A`` sobre el cuerpo ``F``.


.. _GeneralisedEigenspaces:

GeneralisedEigenspaces
~~~~~~~~~~~~~~~~~~~~~~~~

- ``GeneralisedEigenspaces(F, A)`` (operación)

- ``GeneralizedEigenspaces(F, A)`` (operación)

Los espacios propios generalizados de la matriz ``A`` sobre el cuerpo ``F``.


.. _Eigenvalues:

Eigenvalues
~~~~~~~~~~~~~~~~

- ``Eigenvalues(F, A)`` (operación)

Los valores propios de la matriz ``A`` sobre el cuerpo ``F``.


.. _Eigenspaces:

Eigenspaces
~~~~~~~~~~~~~

- ``Eigenspaces(F, A)`` (operación)

Los espacios propios de la matriz ``A`` sobre el cuerpo ``F``.


.. _Eigenvectors:

Eigenvectors
~~~~~~~~~~~~~

- ``Eigenvectors(F, A)`` (operación)

Los vectores propios de la matriz ``A`` sobre el cuerpo ``F``.


Elementary Divisors
---------------------


.. _ElementaryDivisorsMat:

ElementaryDivisorsMat
~~~~~~~~~~~~~~~~~~~~~~

- ``ElementaryDivisorsMat([ring, ]mat)`` (operación)

- ``ElementaryDivisorsMatDestructive(ring, mat)`` (función)

devuelve una lista de los divisores elementales, es decir, el único d con ``d[i]`` divide ``d[i + 1]`` y ``mat`` es equivalente a una matriz diagonal con los elementos ``d[i]`` en la diagonal. Las operaciones se realizan sobre el anillo euclidiano, que debe contener todas las entradas de la matriz. Por razones de compatibilidad, se puede omitir y por defecto es ``DefaultRing`` de las entradas de la matriz.

La función ``ElementaryDivisorsMatDestructive`` produce el mismo resultado pero en el proceso puede destruir el contenido de ``mat``.

.. code-block:: gap
    
    gap> mat:=[[1,2,3],[4,5,6],[7,8,9]];;
    gap> ElementaryDivisorsMat(mat);
    [ 1, 3, 0 ]
    gap> x:=Indeterminate(Rationals,"x");;
    gap> mat:=mat*One(x)-x*mat^0;
    [ [ -x+1, 2, 3 ], [ 4, -x+5, 6 ], [ 7, 8, -x+9 ] ]
    gap> ElementaryDivisorsMat(PolynomialRing(Rationals,1),mat);
    [ 1, 1, x^3-15*x^2-18*x ]
    gap> mat:=KroneckerProduct(CompanionMat((x-1)^2),
    > CompanionMat((x^3-1)*(x-1)));;
    gap> mat:=mat*One(x)-x*mat^0;
    [ [ -x, 0, 0, 0, 0, 0, 0, 1 ], [ 0, -x, 0, 0, -1, 0, 0, -1 ],
    [ 0, 0, -x, 0, 0, -1, 0, 0 ], [ 0, 0, 0, -x, 0, 0, -1, -1 ],
    [ 0, 0, 0, -1, -x, 0, 0, -2 ], [ 1, 0, 0, 1, 2, -x, 0, 2 ],
    [ 0, 1, 0, 0, 0, 2, -x, 0 ], [ 0, 0, 1, 1, 0, 0, 2, -x+2 ] ]
    gap> ElementaryDivisorsMat(PolynomialRing(Rationals,1),mat);
    [ 1, 1, 1, 1, 1, 1, x-1, x^7-x^6-2*x^4+2*x^3+x-1 ]


.. _ElementaryDivisorsTransformationsMat:

ElementaryDivisorsTransformationsMat
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- ``ElementaryDivisorsTransformationsMat([ring, ]mat)`` (operación)

- ``ElementaryDivisorsTransformationsMatDestructive(ring, mat)`` (función)

``ElementaryDivisorsTransformations``, además de las tareas realizadas por ElementaryDivisorsMat_, también calcula la transformación de matrices. Devuelve un registro con componentes normal (una matriz :math:`S`), rowtrans (una matriz :math:`P`) y coltrans (una matriz :math:`Q`) tal que :math:`PAQ = S`. Las operaciones se realizan sobre el anillo euclidiano, que debe contener todas las entradas de la matriz. Por razones de compatibilidad, se puede omitir y por defecto es ``DefaultRing`` de las entradas de la matriz.

La función ``ElementaryDivisorsTransformationsMatDestructive`` produce el mismo resultado pero en el proceso destruye el contenido de ``mat``.

.. code-block:: gap
    
    gap> x:=Indeterminate(Rationals,"x");;
    gap> mat:=KroneckerProduct(CompanionMat((x-1)^2),CompanionMat((x^3-1)*(x-1)));;
    gap> mat:=mat*One(x)-x*mat^0;
    [ [ -x, 0, 0, 0, 0, 0, 0, 1 ], [ 0, -x, 0, 0, -1, 0, 0, -1 ], [ 0, 0, -x, 0, 0, -1, 0, 0 ],
      [ 0, 0, 0, -x, 0, 0, -1, -1 ], [ 0, 0, 0, -1, -x, 0, 0, -2 ], [ 1, 0, 0, 1, 2, -x, 0, 2 ],
      [ 0, 1, 0, 0, 0, 2, -x, 0 ], [ 0, 0, 1, 1, 0, 0, 2, -x+2 ] ]
    gap> t:=ElementaryDivisorsTransformationsMat(PolynomialRing(Rationals,1),mat);
    rec( coltrans := [ [ 0, 0, 0, 0, 0, 0, 1/6*x^2-7/9*x-1/18, -3*x^3-x^2-x-1 ],
          [ 0, 0, 0, 0, 0, 0, -1/6*x^2+x-1, 3*x^3-3*x^2 ],
          [ 0, 0, 0, 0, 0, 1, -1/18*x^4+1/3*x^3-1/3*x^2-1/9*x, x^5-x^4+2*x^2-2*x ],
          [ 0, 0, 0, 0, -1, 0, -1/9*x^3+1/2*x^2+1/9*x, 2*x^4+x^3+x^2+2*x ],
          [ 0, -1, 0, 0, 0, 0, -2/9*x^2+19/18*x, 4*x^3+x^2+x ],
          [ 0, 0, -1, 0, 0, -x, 1/18*x^5-1/3*x^4+1/3*x^3+1/9*x^2, -x^6+x^5-2*x^3+2*x^2 ],
          [ 0, 0, 0, -1, x, 0, 1/9*x^4-2/3*x^3+2/3*x^2+1/18*x, -2*x^5+2*x^4-x^2+x ],
          [ 1, 0, 0, 0, 0, 0, 1/6*x^3-7/9*x^2-1/18*x, -3*x^4-x^3-x^2-x ] ],
      normal := [ [ 1, 0, 0, 0, 0, 0, 0, 0 ], [ 0, 1, 0, 0, 0, 0, 0, 0 ], [ 0, 0, 1, 0, 0, 0, 0, 0 ],
          [ 0, 0, 0, 1, 0, 0, 0, 0 ], [ 0, 0, 0, 0, 1, 0, 0, 0 ], [ 0, 0, 0, 0, 0, 1, 0, 0 ],
          [ 0, 0, 0, 0, 0, 0, x-1, 0 ], [ 0, 0, 0, 0, 0, 0, 0, x^7-x^6-2*x^4+2*x^3+x-1 ] ],
      rowtrans := [ [ 1, 0, 0, 0, 0, 0, 0, 0 ], [ 1, 1, 0, 0, 0, 0, 0, 0 ], [ 0, 0, 1, 0, 0, 0, 0, 0 ],
          [ 1, 0, 0, 1, 0, 0, 0, 0 ], [ -x+2, -x, 0, 0, 1, 0, 0, 0 ],
          [ 2*x^2-4*x+2, 2*x^2-x, 0, 2, -2*x+1, 0, 0, 1 ],
          [ 3*x^3-6*x^2+3*x, 3*x^3-2*x^2, 2, 3*x, -3*x^2+2*x, 0, 1, 2*x ],
          [ 1/6*x^8-7/6*x^7+2*x^6-4/3*x^5+7/3*x^4-4*x^3+13/6*x^2-7/6*x+2,
              1/6*x^8-17/18*x^7+13/18*x^6-5/18*x^5+35/18*x^4-31/18*x^3+1/9*x^2-x+2,
              1/9*x^5-5/9*x^4+1/9*x^3-1/9*x^2+14/9*x-1/9, 1/6*x^6-5/6*x^5+1/6*x^4-1/6*x^3+11/6*x^2-1/6*x,
              -1/6*x^7+17/18*x^6-13/18*x^5+5/18*x^4-35/18*x^3+31/18*x^2-1/9*x+1, 1,
              1/18*x^5-5/18*x^4+1/18*x^3-1/18*x^2+23/18*x-1/18,
              1/9*x^6-5/9*x^5+1/9*x^4-1/9*x^3+14/9*x^2-1/9*x ] ] )
    gap> t.rowtrans*mat*t.coltrans;
    [ [ 1, 0, 0, 0, 0, 0, 0, 0 ], [ 0, 1, 0, 0, 0, 0, 0, 0 ], [ 0, 0, 1, 0, 0, 0, 0, 0 ], 
      [ 0, 0, 0, 1, 0, 0, 0, 0 ], [ 0, 0, 0, 0, 1, 0, 0, 0 ], [ 0, 0, 0, 0, 0, 1, 0, 0 ],
      [ 0, 0, 0, 0, 0, 0, x-1, 0 ], [ 0, 0, 0, 0, 0, 0, 0, x^7-x^6-2*x^4+2*x^3+x-1 ] ]
    gap>


.. _DiagonalizeMat:

DiagonalizeMat
~~~~~~~~~~~~~~~

- ``DiagonalizeMat(ring, mat)`` (operación)

trae la matriz mutable ``mat``, considerado como una matriz sobre anillo ``ring``, en forma diagonal mediante operaciones elementales de fila y columna.

.. code-block:: gap
    
    gap> m:=[[1,2],[2,1]];;
    gap> DiagonalizeMat(Integers,m);
    gap> m;
    [ [ 1, 0 ], [ 0, 3 ] ]
    gap>


.. _Echelonized-Matrices:

Matrices Escalonadas
-----------------------

.. _SemiEchelonMat:

SemiEchelonMat
~~~~~~~~~~~~~~~~~~

- ``SemiEchelonMat(mat)`` (atributo)

Una matriz sobre un cuerpo :math:`\mathbb{F}` está en forma de semi-escalón si el primer elemento distinto de cero en cada fila es la identidad de :math:`\mathbb{F}`, y todos los valores exactamente debajo de estos pivotes son el cero de :math:`\mathbb{F}`.

SemiEchelonMat_ devuelve un registro que contiene información sobre una forma semiescalonada de matriz la ``math``.

Los componentes de este registro son

- ``vectors``

    lista de vectores de fila, cada uno con el elemento pivote la identidad de :math:`\mathbb{F}`,

- ``heads``

    lista que contiene en la posición ``i``, si es distinto de cero, el número de la fila para la que el elemento pivote está en la columna ``i``.

.. _SemiEchelonMatDestructive:

SemiEchelonMatDestructive
~~~~~~~~~~~~~~~~~~~~~~~~~~

- ``SemiEchelonMatDestructive(mat)`` (operación)

Esto hace lo mismo que ``SemiEchelonMat(mat)``, excepto que puede (y probablemente lo hará) destruir la matriz ``mat``.

.. code-block:: gap
    
    gap> mm:=[[1,2,3],[4,5,6],[7,8,9]];;
    gap> SemiEchelonMatDestructive( mm );
    rec( heads := [ 1, 2, 0 ], vectors := [ [ 1, 2, 3 ], [ 0, 1, 2 ] ] )
    gap> mm;
    [ [ 1, 2, 3 ], [ 0, 1, 2 ], [ 0, 0, 0 ] ]


.. _SemiEchelonMatTransformation:

SemiEchelonMatTransformation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- ``SemiEchelonMatTransformation(mat)`` (atributo)

hace lo mismo que SemiEchelonMat_ pero además almacena la transformación lineal :math:`T` realizada en la matriz. Los componentes adicionales del resultado son

- ``coeffs``
    
    una lista de vectores de coeficientes de la componente de vectores, con respecto a las filas de mat, es decir, ``coeff * mat`` es la componente de vectores.

- ``relations``
    
    una lista de vectores base para el espacio nulo (izquierda) de ``mat``.

.. code-block:: gap
    
    gap> SemiEchelonMatTransformation([[1,2,3],[0,0,1]]);
    rec( coeffs := [ [ 1, 0 ], [ 0, 1 ] ], heads := [ 1, 0, 2 ],
    relations := [ ], vectors := [ [ 1, 2, 3 ], [ 0, 0, 1 ] ] )


.. _SemiEchelonMats:

SemiEchelonMats
~~~~~~~~~~~~~~~~~

- ``SemiEchelonMats(mats)`` (operation)

Una lista de matrices sobre un cuerpo :math:`\mathbb{F}` está en forma de semiescalón si la lista de vectores de fila obtenida al concatenar las filas de cada matriz es una matriz semiescalonada (ver SemiEchelonMat_).

SemiEchelonMats_ devuelve un registro que contiene información acerca de una forma semiescalonada de las tablas de lista de matrices.

Los componentes de este registro son

- ``vectors``

    lista de matrices, cada una con el elemento pivote la identidad de ``F``,

- ``heads``

    matriz que contiene en la posición ``[i, j]``, si es diferente de cero, el número de la matriz que tiene el elemento pivote en esta posición


.. _SemiEchelonMatsDestructive:

SemiEchelonMatsDestructive
~~~~~~~~~~~~~~~~~~~~~~~~~~~

- ``SemiEchelonMatsDestructive(mats)`` (operación)

Hace lo mismo que SemiEchelonmats_, excepto que puede destruir su argumento. Por lo tanto, el argumento debe ser una lista de matrices re mutables.


Matrices como base de un espacio de filas
----------------------------------------------------------------------

.. _BaseMat:

BaseMat
~~~~~~~~~~

- ``BaseMat(mat)`` (atributo)

devuelve una base para el espacio de fila generado por las filas de ``mat`` en forma de una matriz inmutable.

.. _BaseMatDestructive:

BaseMatDestructive
~~~~~~~~~~~~~~~~~~~

- ``BaseMatDestructive(mat)`` (operación)

Hace lo mismo que BaseMat_, con la diferencia de que puede destruir de la matriz ``mat``. La matriz ``mat`` debe ser mutable.

.. code-block:: gap
    
    gap> mat:=[[1,2,3],[4,5,6],[7,8,9]];;
    gap> BaseMat(mat);
    [ [ 1, 2, 3 ], [ 0, 1, 2 ] ]
    gap> mm:= [[1,2,3],[4,5,6],[5,7,9]];;
    gap> BaseMatDestructive( mm );
    [ [ 1, 2, 3 ], [ 0, 1, 2 ] ]
    gap> mm;
    [ [ 1, 2, 3 ], [ 0, 1, 2 ], [ 0, 0, 0 ] ]


.. _BaseOrthogonalSpaceMat:

- ``BaseOrthogonalSpaceMat(mat)`` (atributo)

Sea :math:`V` el espacio de fila generado por las filas de ``mat`` (sobre cualquier campo que contenga todas las entradas de ``mat``). ``BaseOrthogonalSpaceMat(mat)`` calcula una base del espacio ortogonal de :math:`V`.

No es necesario que las filas de estera sean linealmente independientes.

.. _SumIntersectionMat:

SumIntersectionMat
~~~~~~~~~~~~~~~~~~~

- ``SumIntersectionMat(M1, M2)`` (operación)

realiza el algoritmo de Zassenhaus para calcular las bases de la suma y la intersección de los espacios generados por las filas de las matrices ``M1``, ``M2``.

Devuelve una lista de longitud :math:`2`, en la primera posición una base de la suma, en la segunda posición una base de la intersección. Ambas bases están en forma de semi-escalón (ver :ref:`Echelonized-Matrices`).

.. code-block:: gap
    
    gap> SumIntersectionMat(mat,[[2,7,6],[5,9,4]]);
    [ [ [ 1, 2, 3 ], [ 0, 1, 2 ], [ 0, 0, 1 ] ], [ [ 1, -3/4, -5/2 ] ] ]


.. _BaseSteinitzVectors:

BaseSteinitzVectors
~~~~~~~~~~~~~~~~~~~~~

- ``BaseSteinitzVectors(bas, mat)`` (función)

encuentre vectores que extiendan ``mat`` hasta una base que abarque el intervalo de bas. Tanto bas como ``mat`` deben ser matrices de rango completo (fila). Devuelve un registro con los siguientes componentes:

``subspace``

    es una base del espacio atravesado por ``mat`` en forma triangular superior con los primeros en todos los escalones y ceros por encima de estos.

``factorspace``

    es una lista de vectores que se extienden en forma triangular superior.

``factorzero``
    
    es un vector cero.

``heads``
    
    es una lista de números enteros que se pueden utilizar para descomponer vectores en los vectores base. La ``i``-ésima entrada indica el vector que da un escalón en la posición ``i``. Un número negativo indica un escalón en el subespacio, un número positivo un escalón en el complemento, el valor absoluto da la posición del vector en las listas subespacio y factorespacio.

.. code-block:: gap
    
    gap> BaseSteinitzVectors(IdentityMat(3,1),[[11,13,15]]);
    rec( factorspace := [ [ 0, 1, 15/13 ], [ 0, 0, 1 ] ], factorzero := [ 0, 0, 0 ], heads := [ -1, 1, 2 ],
      subspace := [ [ 1, 13/11, 15/11 ] ] )
    gap>


Matrices Triangulares
----------------------

.. _DiagonalOfMat:

DiagonalOfMat
~~~~~~~~~~~~~~

- ``DiagonalOfMat(mat)`` (función)

devuelve la diagonal de la matriz ``mat``. Si ``mat`` no es una matriz cuadrada, el resultado tiene la misma longitud que las filas de ``mat`` y se rellena con ceros si ``mat`` tiene menos filas que columnas.

.. code-block:: gap
    
    gap> DiagonalOfMat([[1,2,3],[4,5,6]]);
    [ 1, 5, 0 ]

.. _UpperSubdiagonal:

UpperSubdiagonal
~~~~~~~~~~~~~~~~~

- ``UpperSubdiagonal(mat, pos)`` (operación)

devuelve una lista mutable que contiene las entradas de la ``pos``-ésima subdiagonal superior de ``mat``.

.. code-block:: gap
    
    gap> mat:=[[1,2,3],[4,5,6],[7,8,9]];;
    gap> mat;
    [ [ 1, 2, 3 ], [ 4, 5, 6 ], [ 7, 8, 9 ] ]
    gap> Display(mat);
    [ [  1,  2,  3 ],
      [  4,  5,  6 ],
      [  7,  8,  9 ] ]
    gap> UpperSubdiagonal(mat,1);
    [ 2, 6 ]
    gap> UpperSubdiagonal(mat,2);
    [ 3 ]
    gap> UpperSubdiagonal(mat,3);
    [  ]



.. _DepthOfUpperTriangularMatrix:

DepthOfUpperTriangularMatrix
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- ``DepthOfUpperTriangularMatrix(mat)`` (atributo)

Si ``mat`` es una matriz triangular superior, este atributo devuelve el índice de la primera diagonal distinta de cero.

.. code-block:: gap
    
    gap> DepthOfUpperTriangularMatrix([[0,1,2],[0,0,1],[0,0,0]]);
    1
    gap> DepthOfUpperTriangularMatrix([[0,0,2],[0,0,0],[0,0,0]]);
    2