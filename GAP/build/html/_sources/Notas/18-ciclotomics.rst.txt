Números Ciclotómicos
====================

GAP admite cálculos en campos de extensión abelianos del campo numérico racional :math:`\mathbb{Q}`, es decir, campos con un grupo de Galois abeliano sobre :math:`\mathbb{Q}`. Estos campos son subcampos de campos ciclotómicos :math:`\mathbb{Q} (e^{n})` donde :math:`\mathbb{Q} (e^{n}) = exp (2 \pi i / n)` es un complejo primitivo :math:`n`-ésima raíz de la unidad. Los elementos de estos campos se denominan ciclotómica.

La información relativa a las operaciones para los dominios de la ciclotómica, por ejemplo, ciertas bases integrales de los campos de la ciclotómica, se puede encontrar en el Capítulo 60 del manual ofivial de GAP. Para operaciones más generales que toman una extensión de campo como un argumento –posiblemente opcional–, por ejemplo, ``Trace`` o ``Coefficients``, funciones que no desarrollaremos en este tutorial.


Operations para Números Ciclotómicos
------------------------------------

.. _E:

E
~~~

- ``E(n)`` (operation)

E_ devuelve la raíz :math:`n`-ésima primitiva de la unidad :math:`\mathbb{Q} (e^{n}) = exp (2 \pi i / n)`. La ciclotómica generalmente se ingresa como sumas de raíces de unidad, con coeficientes racionales, y la ciclotómica irracional se muestra de esa manera. (Para ciclotómicas especiales, consulte 18.4.)

.. code-block:: gap
    
    gap> E(9);
    -E(9)^4-E(9)^7
    gap> E(9)^3;
    E(3)
    gap> E(6);
    -E(3)^2
    gap> E(12)/3;
    -1/3*E(12)^7
    gap>


Se utiliza una base particular para expresar la ciclotómica; tenga en cuenta que ``E(9)`` no es un elemento base, como muestra el ejemplo anterior.

.. _IsCyclotomic:

IsCyclotomic
~~~~~~~~~~~~~

- IsCyclotomic(obj) (Category)

- IsCyc(obj) (Category)

Todos los objetos de la familia ``CyclotomicsFamily`` pertenecen a la categoría IsCyclotomic_. Esto cubre enteros, racionales, ciclotómica propia, el objeto ``infinity`` (18.2.1) y las incógnitas (ver Capítulo 74).

Todos estos objetos excepto el ``infinity`` (18.2.1) y las incógnitas se encuentran también en la categoría ``IsCyc`` el ``infinity`` (18.2.1) se encuentra en (y se puede detectar desde) la categoría IsInfinity (18.2.1), y las incógnitas se encuentran en ``IsUnknown``

.. code-block:: gap
    
    gap> # Ejemplo función IsCyclotomic
    gap> IsCyclotomic(0);
    true
    gap> IsCyclotomic(1/2*E(3));
    true
    gap> IsCyclotomic( infinity );
    true
    gap> # Ejemplo función IsCyc
    gap> IsCyc(0);
    true
    gap> IsCyc(1/2*E(3));
    true
    gap> IsCyc( infinity );
    false
    gap>



.. _AbsoluteValue:

AbsoluteValue
~~~~~~~~~~~~~~

- ``AbsoluteValue(cyc)`` (attribute)

devuelve el valor absoluto de un número ciclotómico ``cyc``. Por el momento solo existen métodos para números racionales.

.. code-block:: gap
    
    gap> AbsoluteValue(-3);
    3

