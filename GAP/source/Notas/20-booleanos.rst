Booleanos
==========

Los dos valores booleanos principales son ``true`` y ``false``. Representan los valores lógicos del mismo nombre. Aparecen como valores de las condiciones en sentencias ``if`` y bucles ``while``. Los booleanos también son importantes como valores de retorno de ``filters`` (ver 13.2) como IsFinite (30.4.2) e IsBool_ (20.1.1). Tenga en cuenta que es una convención que el nombre de una función que devuelve ``true`` o ``false`` según el resultado, comience con ``Is``.

Por razones técnicas, también el valor fail_ se considera un booleano.

IsBool (Filter)
-------------------------

.. _IsBool:

IsBool
~~~~~~~~~~~~

La categoría ``IsBool(obj)`` prueba si ``obj`` es ``true``, ``falso`` o ``fail``.

.. code-block:: gap
    
    gap> IsBool( true ); IsBool( false ); IsBool( 17 );
    true
    true
    false



Fail (Variable)
----------------------------------

.. _fail:

fail
~~~~~~~~

fail (global variable)

El valor fail_ se utiliza para indicar situaciones en las que no se pudo realizar una operación para los argumentos dados, ya sea por deficiencias de los argumentos o por restricciones en la implementación o computabilidad. Entonces, por ejemplo, la Posición devolverá un error si el punto buscado no está en la lista.

fail_ es simplemente un objeto que es diferente de cualquier otro objeto que él mismo.

Por razones técnicas, fail_ es un valor booleano. Pero tenga en cuenta que no se puede usar fail para formar expresiones booleanas con and_, or_, y not_, y no puede aparecer en listas booleanas.

Comparación de Booleanos
----------------------------------

Igualdad y desigualdad de Booleans
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``bool1 = bool2``

``bool1 <> bool2``

- El operador de igualdad ``=`` se evalúa como ``true`` si los dos valores booleanos ``bool1`` y ``bool2`` son iguales, es decir, ambos son verdaderos o ambos son falsos o ambos ``fail``, y ``false`` en caso contrario.

- El operador de desigualdad ``<>`` se evalúa como ``true`` si los dos valores booleanos ``bool1``, ``bool2`` son diferentes y ``false`` en caso contrario. Esta operación también se llama *exclusiva o*, porque su valor es ``true`` si exactamente uno de ``bool1`` o ``bool2`` es verdadero.

Puede comparar valores booleanos con objetos de otros tipos. Por supuesto que nunca son iguales.

.. code-block:: gap

    gap> true = false;
    false
    gap> false = (true = fail);
    true
    gap> true <> 17;
    true

Ordenación de Booleanos
~~~~~~~~~~~~~~~~~~~~~~~

``bool1 < bool2``

El orden de los valores booleanos se define mediante ``true < false < fail``. Para la comparación de valores booleanos con otros objetos GAP, consulte la Sección 4.12.

.. code-block:: gap
    
    gap> true < false; fail >= false;
    true
    true

Operaciones para Booleanos
--------------------------

Las siguientes operaciones booleanas solo se aplican a ``true`` y ``false``.

.. _or:

Disjunción Lógica
~~~~~~~~~~~~~~~~~

``bool1 or bool2``

El operador lógico or_ se evalúa como verdadero si al menos uno de los dos operandos booleanos ``bool1`` y ``bool2`` es verdadero, y ``false`` en caso contrario. or_ primero evalúa ``bool1``. Si el valor no es ni verdadero ni falso, se indica un error. Si el valor es verdadero, entonces o devuelve ``true`` sin evaluar ``bool2``. Si el valor es falso, entonces or_ evalúa ``bool2``.

Nuevamente, si el valor no es ni verdadero ni falso, se indica un error. De lo contrario, or_ devuelve el valor de ``bool2``. Esta evaluación en cortocircuito es importante si el valor de ``bool1`` es verdadero y la evaluación de ``bool2`` tomaría mucho tiempo or_ causaría un error.

- or_ es asociativo, es decir, se permite escribir ``b1 or b2 or b3``, que se interpreta como ``(b1 or b2) or b3``. or_ tiene la precedencia más baja de los operadores lógicos. Todos los operadores lógicos tienen menor precedencia que los operadores de comparación ``=``, ``<``, ``in``, etc.

.. code-block:: gap
    
    gap> true or false;
    true
    gap> false or false;
    false
    gap> i := -1;; l := [1,2,3];;
    gap> if i <= 0 or l[i] = false then # this does not cause an error,
    > Print("aha\n"); fi; # because ‘l[i]’ is not evaluated
    aha

.. _and:

Conjunción Lógica
~~~~~~~~~~~~~~~~~

``bool1 and bool2``

``fil1 and fil2``

El operador lógico and_ se evalúa como ``true`` si ambos operandos booleanos ``bool1``, ``bool2`` son verdaderos y como ``false`` en caso contrario.

- and_ primero evalúa ``bool1``. Si el valor no es ni verdadero ni falso, se indica un error. Si el valor es falso, entonces y devuelve ``false`` sin evaluar ``bool2``. Si el valor es ``true``, entonces y evalúa ``bool2``. Nuevamente, si el valor no es ni verdadero ni falso, se indica un error. De lo contrario y devuelve el valor de ``bool2``. Esta evaluación en cortocircuito es importante si el valor de ``bool1`` es falso y la evaluación de ``bool2`` llevaría mucho tiempo o provocaría un error.

- and_ es asociativo, es decir, se permite escribir ``b1 and b2 and b3``, que se interpreta como ``(b1 and b2) and b3``. y tiene mayor precedencia que el operador lógico or_, pero menor que el operador lógico no unario. Todos los operadores lógicos tienen menor precedencia que los operadores de comparación ``=``, ``<``, ``in``, etc.

.. code-block:: gap
    
    gap> true and false;
    false
    gap> true and true;
    true
    gap> false and 17; # does not cause error, because 17 is never looked at
    false

y también se puede aplicar a ``filters``. Devuelve un filtro que cuando se aplica a algún argumento ``x``, prueba ``fil1 (x) and fil2 (x)``.

.. code-block:: gap
    
    gap> andfilt:= IsPosRat and IsInt;;
    gap> andfilt( 17 ); andfilt( 1/2 );
    true
    false

.. _not:

Negación Lógica
---------------

``not bool``

El operador lógico not_ devuelve ``true`` si el valor booleano ``bool`` es falso y ``true`` en caso contrario.

Se indica un error si ``bool`` no se evalúa como ``true`` o ``false``.

not_ tiene mayor precedencia que los otros operadores lógicos, or_ y and_. Todos los operadores lógicos tienen menor precedencia que los operadores de comparación ``=``, ``<``, ``in``, etc.

.. code-block:: gap
    
    gap> true and false;
    false
    gap> not true;
    false
    gap> not false;
    true