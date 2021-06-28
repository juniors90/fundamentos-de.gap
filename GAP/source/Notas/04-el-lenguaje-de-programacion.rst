.. role:: underline
    :class: underline


El Lenguaje de Programación
============================

Este capítulo describe el lenguaje de programación **GAP**. Debería permitinos, en principio, predecir el resultado de todas y cada una de las entradas. Para saber de qué estamos hablando, primero tenemos que mirar más de cerca el proceso de interpretación y las diversas representaciones de datos involucradas.

.. _descripcion-general-del-lenguaje:

Descripción general del Lenguaje
----------------------------------

Primero tenemos la entrada a **GAP**, dada como una cadena de caracteres. La forma en que esos caracteres ingresan a **GAP** depende del sistema operativo, por ejemplo, pueden ingresarse en una terminal, pegarse con un mouse en una ventana o leerse desde un archivo. El mecanismo no importa. Esta representación de expresiones por caracteres se denomina representación externa de la expresión. Cada expresión tiene al menos una representación externa que se puede ingresar para obtener exactamente esta expresión.

La entrada, es decir, la representación externa, se transforma en un proceso llamado lectura en una representación interna. En este punto se analiza la entrada y las entradas que no son representaciones externas legales, de acuerdo con las reglas que se dan a continuación, se rechazan como errores. Por lo general, esas reglas se denominan :underline:`sintaxis de un lenguaje de programación`.

La representación interna creada por la lectura se llama expresión o declaración. Más adelante distinguiremos entre esos dos términos. Sin embargo, por ahora los usaremos indistintamente. No importa la forma exacta de la representación interna. Podría ser una cadena de caracteres igual a la representación externa, en cuyo caso la lectura solo necesitaría verificar errores. Podría ser una serie de instrucciones de máquina para el procesador en el que se ejecuta GAP, en cuyo caso la lectura se llamaría más apropiadamente compilación. De hecho, es una estructura en forma de árbol.

Una vez que se ha leído la entrada, se vuelve a transformar en un proceso llamado evaluación o ejecución. Más adelante también distinguiremos entre esos dos términos, pero por el momento los usaremos indistintamente. El nombre insinúa la naturaleza de este proceso, reemplaza una expresión con el valor de la expresión. Esto funciona de forma recursiva, es decir, para evaluar una expresión, primero se evalúan las subexpresiones y luego se calcula el valor de la expresión a partir de esos valores de acuerdo con las reglas que se indican a continuación. Por lo general, esas reglas se denominan semántica de un lenguaje de programación.

El resultado de la evaluación se llama, como es lógico, valor. Una vez más, la forma en que dicho valor se representa internamente no importa. De hecho, es nuevamente una estructura en forma de árbol.

El último proceso se llama impresión. Toma el valor producido por la evaluación y crea una representación externa, es decir, nuevamente una cadena de caracteres. Lo que hagas con esta representación externa depende de nosotros. Puede mirarlo, pegarlo con el mouse en otra ventana o escribirlo en un archivo.

Veamos un ejemplo para aclarar esto. Suponga que escribe la siguiente cadena de *8* caracteres

.. code-block:: gap

    1 + 2 * 3;

**GAP** toma esta representación externa y crea una representación interna en forma de árbol, que podemos representar de la siguiente manera

.. code-block:: none

        +
       / \
      1   *
         / \
        2   3

Luego se evalúa esta expresión. Para hacer esto, GAP primero evalúa la subexpresión derecha ``2 * 3``. Nuevamente, para hacer esto **GAP** primero evalúa sus subexpresiones ``2`` y ``3``. Sin embargo son tan simples que tienen su propio valor, decimos que son autoevaluables. Una vez hecho esto, la regla para ``*`` nos dice que el valor es el producto de los valores de las dos subexpresiones, que en este caso es claramente ``6``.

Combinando esto con el valor del operando izquierdo del ``+``, que también se autoevalúa, nos da el valor de toda la expresión ``7``. Esto luego se imprime, es decir, se convierte en la representación externa que consta del carácter único ``7``.

De esta manera podemos predecir el resultado de cada entrada cuando conocemos las reglas sintácticas que gobiernan el proceso de lectura y las reglas semánticas que nos dicen para cada expresión cómo se calcula su valor en términos de los valores de las subexpresiones.

    - Las reglas sintácticas se dan en las secciones :ref:`estructura-lexica`, 4.3, 4.4, :ref:`palabras-reservadas` y 4.6,
    - las reglas semánticas se dan en las secciones 4.7, 4.8, 4.11, 4.12, 4.13, 4.14, 4.15, 4.16, 4.17, 4.18, 4.19, 4.20, 4.23 y
    - los capítulos que describen los tipos de datos individuales.

.. _estructura-lexica:

Estructura léxica
-------------------------

La mayor parte de la entrada de **GAP** consta de secuencias de los siguientes caracteres:

    - Dígitos,
    - Letras mayúsculas y minúsculas,
    - SPACE,
    - TAB,
    - NEWLINE,
    - RETURN y
    - los caracteres especiales
        
        .. code-block:: none
        
            " ' ( ) * + , - # . / : ; < = > ~ [ \ ] ^ _ { } !

Es posible usar otros caracteres en identificadores escapándolos con barras invertidas, pero no recomendamos usar esta función. Dentro de las cadenas (ver Sección :ref:`simbolos` y capítulo 27) y comentarios (ver 4.4) se permite el juego completo de caracteres admitido por la computadora.

.. _simbolos:

Símbolos
----------------

El proceso de lectura, es decir, de ensamblar la entrada en expresiones, tiene un subproceso, llamado escaneo, que ensambla los caracteres en símbolos.
     
    - Un *símbolo* es una secuencia de caracteres que forman una unidad léxica. El conjunto de símbolos consta de palabras reservadas, identificadores, cadenas, números enteros y símbolos de operador y delimitador.
    
    - Una *palabra reservada* es una palabra propia del lenguaje, que se reserva para utilizarse en la escritua del código. (ver :ref:`palabras-reservadas`).
    
    - Un *identificador* es una secuencia de letras, dígitos y guiones bajos (u otros caracteres escapados por barras diagonales inversas) que contiene al menos un no dígito y no es una palabra reservada (ver 4.6).
    
    - Un *número entero* es una secuencia de dígitos (ver 14), posiblemente precedidos por caracteres de signo ``-`` y ``+``.
    
    - Una *cadena* es una secuencia de caracteres arbitrarios entre comillas dobles (ver 27).
    
    - Los símbolos de operador y delimitador son
    
    .. code-block:: gap
        
        + - * / ^ ~ !. = <> < <= > >= ![ := . .. -> , ; !{ [ ] { } ( ) :

Tenga en cuenta también que durante el proceso de escaneo se eliminan todos los espacios en blanco (ver :ref:`espacios-en-blanco`).

.. _espacios-en-blanco:

Espacios en blanco
----------------------------------

Los caracteres ``SPACE``, ``TAB``, ``NEWLINE`` y ``RETURN`` se denominan caracteres de espacio en blanco. El espacio en blanco se utiliza según sea necesario para separar símbolos léxicos, como números enteros, identificadores o palabras reservadas. Por ejemplo, ``Thorondor`` es un identificador único, mientras que ``Th`` u ``ondor`` es la palabra reservada o entre los dos identificadores ``Th`` y ``ondor``. Los espacios en blanco pueden aparecer entre dos símbolos, pero no dentro de un símbolo. Dos o más caracteres de espacio en blanco adyacentes equivalen a un solo espacio en blanco. Aparte del papel de separador de símbolos, los espacios en blanco son insignificantes. Los caracteres de espacio en blanco también pueden aparecer dentro de una cadena, donde son significativos. Los caracteres de espacio en blanco también deben usarse libremente para mejorar la legibilidad.

Un comentario comienza con el carácter ``#``, que a veces se denomina agudo o sombreado, y continúa hasta el final de la línea en la que aparece el carácter de comentario. El comentario completo, incluidos ``#`` y el carácter ``NEWLINE`` se trata como un solo espacio en blanco. Dentro de una cadena, el carácter de comentario ``#`` pierde su función y es solo un carácter ordinario.

Por ejemplo, la siguiente declaración

.. code-block:: gap

    if i<0 then a:=-i;else a:=i;fi;

es equivalente a

.. code-block:: gap

    if i < 0 then    # if i is negative
        a := -i;     # take its additive inverse
    else             # otherwise
        a := i;      # take itself
    fi;

(que por cierto muestra que es posible escribir comentarios superfluos). Sin embargo, la primera declaración *no* es equivalente a

.. code-block:: gap

    ifi<0thena:=-i;elsea:=i;fi;

ya que la palabra reservada ``if`` debe estar separada del identificador ``i`` por un espacio en blanco, y de manera similar, ``then`` y ``a``, y ``else`` y ``a`` deben estar separados.

.. _palabras-reservadas:

Palabras Reservadas
----------------------------------

Las palabras reservadas se utilizan para denotar operaciones especiales o forman parte de declaraciones. No deben utilizarse como identificadores. La lista de palabras reservadas está contenida en el componente ``GAPInfo.Keywords`` del registro ``GAPInfo``. Mostraremos cómo imprimirlo en una bonita tabla, demostrando al mismo tiempo algunas técnicas de manipulación de listas:

.. code-block:: gap
    :caption: palabras reservadas
    :name: palabras_reservadas

    gap> keys:=SortedList( GAPInfo.Keywords );;
    gap> l:=Length( keys );;
    gap> arr:= List( [ 0 .. Int( l/4 )-1 ], i-> keys{ 4*i + [ 1 .. 4 ] } );;
    gap> if l mod 4 <> 0 then
    >     Add( arr, keys{[ 4*Int(l/4) + 1 .. l ]} );
    > fi;
    gap> Length( keys );
    35
    gap> PrintArray( arr );
    [ [         Assert,           Info,        IsBound,           QUIT ],
      [  TryNextMethod,         Unbind,            and,         atomic ],
      [          break,       continue,             do,           elif ],
      [           else,            end,          false,             fi ],
      [            for,       function,             if,             in ],
      [          local,            mod,            not,             od ],
      [             or,           quit,       readonly,      readwrite ],
      [            rec,         repeat,         return,           then ],
      [           true,          until,          while ] ]
    gap>

Tenga en cuenta que (casi) todas las palabras reservadas están escritas en minúsculas y distinguen entre mayúsculas y minúsculas. Por ejemplo, ``else`` es una palabra reservada; ``Else``, ``eLsE``, ``ELSE`` y así sucesivamente son identificadores ordinarios. Las palabras reservada no deben contener espacios en blanco, por ejemplo, el ``if`` no es lo mismo que ``elif``.

.. note::

    varios tokens de la lista de palabras reservadas anterior pueden parecer identificadores normales que representan funciones o literales de varios tipos, pero en realidad se implementan como palabras reservadas por razones técnicas. La única consecuencia de esto es que esos identificadores no se pueden reasignar y, en realidad, no tienen objetos de función vinculados a ellos, que podrían asignarse a otras variables o pasarse a funciones. Estas palabras reservadas son ``true``, ``false``, ``Assert (7.5.3)``, ``IsBound (4.8.1)``, ``Unbind (4.8.2)``, ``Info (7.4.5)`` y ``TryNextMethod (78.4.1)``.

Las palabras reservadas atomic, readonly, readwrite no se utilizan en este momento. Están reservados para la versión futura de **GAP** para evitar su uso accidental como identificadores.

.. _identificadores:

Identificadores
----------------------------------


Un *identificador* se usa para referirse a una variable (ver 4.8). Un identificador generalmente consta de letras, dígitos, guiones bajos ``_`` y caracteres "arroba" ``@``, y debe contener al menos un caracter que no sea un dígito. Un identificador termina con el primer carácter que no está en esta clase. Tenga en cuenta que el carácter "arroba" ``@`` se utiliza para implementar espacios de nombres; consulte la Sección 4.10 para obtener más detalles.

Ejemplos de identificadores válidos son

.. code-block::

    a           foo         aLongIdentifier
    hello       Hello       HELLO
    x100        100x        _100
    some_people_prefer_underscores_to_separate_words
    WePreferMixedCaseToSeparateWords
    abc@def

Tenga en cuenta que el caso es significativo, por lo que se distinguen los tres identificadores de la segunda línea.

La barra invertida ``\`` se puede utilizar para incluir otros caracteres en los identificadores; una barra invertida seguida de un carácter es equivalente al carácter, excepto que esta secuencia de escape se considera una letra normal. Por ejemplo

.. code-block::

    G\(2\,5\)

es un identificador, no una llamada a una función ``G``.

Un identificador que comienza con una barra invertida nunca es una palabra reservada, por lo que, por ejemplo, ``\*`` y ``\mod`` son identificadores.

La longitud de los identificadores no está limitada, sin embargo, solo los primeros ``1023`` caracteres son significativos. La secuencia de escape ``\NEWLINE`` se ignora, lo que permite dividir identificadores largos en varias líneas.

.. _IsValidIdentifier:

IsValidIdentifier
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

La función ``IsValidIdentifier( str )`` devuelve ``true`` si la cadena ``str`` formaría un identificador válido que consta de letras, dígitos y guiones bajos; de lo contrario, devuelve ``false``. No comprueba si str contiene caracteres escapados por una barra invertida ``\``.

Tenga en cuenta que el carácter "arroba" se utiliza para implementar espacios de nombres para variables globales en paquetes. Consulte 4.10 para obtener más detalles.


.. _expresiones:

Expresiones
----------------------------------

Una expresión es una construcción que se evalúa como un valor. Las construcciones sintácticas que se ejecutan para producir un efecto secundario y no devuelven ningún valor se denominan declaraciones (ver 4.14). Las expresiones aparecen como el lado derecho de las asignaciones (ver 4.15), como argumentos reales en llamadas a funciones (ver 4.11) y en declaraciones.

Tenga en cuenta que una expresión no es lo mismo que un valor. Por ejemplo, ``1 + 11`` es una expresión, cuyo valor es el entero ``12``. La representación externa de este entero es la secuencia de caracteres ``12``, es decir, esta secuencia se emite si se imprime el entero. Esta secuencia es otra expresión cuyo valor es el entero ``12``. El intérprete realiza el proceso de encontrar el valor de una expresión y se denomina evaluación de la expresión.

Las variables, llamadas a funciones y literales de enteros, permutación, cadena, función, lista y registro (ver 4.8, 4.11, 14, 42, 27, 4.23, 21, 29) son los casos más simples de expresiones.

Las expresiones, por ejemplo, las expresiones simples mencionadas anteriormente, se pueden combinar con los operadores para formar expresiones más complejas. Por supuesto, esas expresiones se pueden combinar con los operadores para formar expresiones aún más complejas. Los operadores se dividen en tres clases.
    
    - Los operadores comparación (ver 4.12 y 30.6). son
    
    +--------+-----------------------------------------------------+
    | ``=``  |                                                     |
    +--------+-----------------------------------------------------+
    | ``<>`` |                                                     |
    +--------+-----------------------------------------------------+
    | ``<``  |                                                     |
    +--------+-----------------------------------------------------+
    | ``<=`` |                                                     |
    +--------+-----------------------------------------------------+
    | ``>``  |                                                     |
    +--------+-----------------------------------------------------+
    | ``>=`` |                                                     |
    +--------+-----------------------------------------------------+
    | ``in`` |                                                     |
    +--------+-----------------------------------------------------+

    - Los operadores aritméticos (ver 4.13) son

    +---------+-----------------------------------------------------+
    | ``+``   |                                                     |
    +---------+-----------------------------------------------------+
    | ``-``   |                                                     |
    +---------+-----------------------------------------------------+
    | ``*``   |                                                     |
    +---------+-----------------------------------------------------+
    | ``/``   |                                                     |
    +---------+-----------------------------------------------------+
    | ``mod`` |                                                     |
    +---------+-----------------------------------------------------+
    | ``^``   |                                                     |
    +---------+-----------------------------------------------------+
    
    - Los operadores lógicos (ver 20.4) son

    +---------+-----------------------------------------------------+
    | ``not`` |  :ref:`not`                                         |
    +---------+-----------------------------------------------------+
    | ``and`` |  :ref:`and`                                         |
    +---------+-----------------------------------------------------+
    | ``or``  |  :ref:`or`                                          |
    +---------+-----------------------------------------------------+

El siguiente ejemplo muestra una expresión muy simple con valor ``4`` y una expresión más compleja.

.. code-block:: gap

    gap> 2 * 2;
    4
    gap> 2 * 2 + 9 = Fibonacci(7) and Fibonacci(13) in Primes;
    true


.. _variable:

Variable
-----------

Una variable es una ubicación en un programa GAP que tiene asignado un valor. Decimos que la variable está vinculada a este valor. Si se consulta una variable, se devuelve con este valor.

.. _argumentos:

Argumentos
~~~~~~~~~~~~

GAP nos permite trabajar con distintos tipos de variables. Una clase especial de variables es la clase de argumentos_ de funciones. Se comportan de manera similar a otras variables, excepto que están vinculadas al valor de los argumentos reales en una llamada de función, que veremos en capítulos más delante.

.. _identificador:

Identificador
~~~~~~~~~~~~~~

Cada variable tiene un nombre que también se llama su identificador_. Esto se debe a que en un ámbito dado un identificador_ identifica una variable única.

Por ejemplo, si realizamos una operación ``7+5;`` esto nos mostrará por consola ``12``. Sin embargo, si queremos utilizar este valor, tenemos que almacenarlo en una variable_ que llamaremos "``x``", cuya entidad básica almacenará el calculo ``7+5;``. En el caso de GAP, esto se realiza con ``:=`` finalizando la sentencia con ``;``. Luego, si consultamos el valor de la variable ``x`` obtenemos ``12``. Si yo asigno un nuevo valor, esta varible se modifica. Veamos todo esto en el siguiente ejemplo.

.. code-block:: gap

    ┌───────┐   GAP 4.10.2 of 19-Jun-2019
    │  GAP  │   https://www.gap-system.org
    └───────┘   Architecture: i686-pc-cygwin-default32-kv3
    Configuration:  gmp 6.1.2, readline
    Loading the library and packages ...
    Packages: AClib 1.3.1, Alnuth 3.1.1, AtlasRep 2.1.0, AutoDoc 2019.05.20, AutPGrp 1.10, Browse 1.8.8,
              CRISP 1.4.4, Cryst 4.1.19, CrystCat 1.1.9, CTblLib 1.2.2, FactInt 1.6.2, FGA 1.4.0,
              Forms 1.2.5, GAPDoc 1.6.2, genss 1.6.5, IO 4.6.0, IRREDSOL 1.4, LAGUNA 3.9.3, orb 4.8.2,
              Polenta 1.3.8, Polycyclic 2.14, PrimGrp 3.3.2, RadiRoot 2.8, recog 1.3.2, ResClasses 4.7.2,
              SmallGrp 1.3, Sophus 1.24, SpinSym 1.5.1, TomLib 1.2.8, TransGrp 2.0.4, utils 0.63
     Try '??help' for help. See also '?copyright', '?cite' and '?authors'
    gap> 7+5;
    12
    gap> x:=7+5;
    12
    gap> x;
    12
    gap> x:=15;
    15
    gap> x;
    15
    gap>



.. _alcance:

Alcance
~~~~~~~~~

Un alcance_ es una parte léxica de un texto de programa.

- Existe el **alcance global** que encierra todo el texto del programa, y ​​hay ámbitos locales que van desde la palabra clave de función, que denota el comienzo de una definición de función, hasta la palabra clave final correspondiente.

- Un **alcance local** introduce nuevas variables, cuyos identificadores se dan en la lista de argumentos formales y la declaración local de la función.

El uso de un identificador_ en un texto de programa se refiere a la variable en el ámbito más interno que tiene este identificador_ como su nombre.

Debido a que este mapeo de identificadores a variables se realiza cuando se lee el programa, no cuando se ejecuta, se dice que GAP tiene alcance léxico. El siguiente ejemplo muestra cómo un identificador se refiere a diferentes variables en diferentes puntos del texto del programa.


.. code-block:: gap

    g := 0; # variable global g
    x := function ( a, b, c )
        local y;
        g := c; # c se refiere al argumento c de la función x
        y := function ( y )
            local d, e, f;
            d := y; # y se refiere al argumento y de la función y
            e := b; # b se refiere al argumento b de la función x
            f := g; # g se refiere a la variable global g
            return d + e + f;
        end;
        return y( a ); # y se refiere a la variable local y de la función x
    end;


Es importante señalar que el concepto de variable en GAP es bastante diferente del concepto de variable en la mayoría de los lenguajes de programación compilados.

En esos lenjuajes, una variable denota un bloque de memoria. El valor de la variable se almacena en este bloque. Entonces, en esos lenguajes, dos variables pueden tener el mismo valor, pero nunca pueden tener valores idénticos, porque denotan diferentes bloques de memoria. Tenga en cuenta que algunos lenguajes tienen el concepto de argumento de referencia. Parece como si dicho argumento y la variable utilizada en la llamada a la función real tuvieran el mismo valor, ya que cambiar el valor del argumento también cambia el valor de la variable utilizada en la llamada a la función real. Pero esto no es así; el argumento de referencia es en realidad un puntero a la variable utilizada en la llamada a la función real, y es el compilador el que inserta suficiente magia para hacer que el puntero sea invisible. Para que esto funcione, el compilador necesita suficiente información para calcular la cantidad de memoria necesaria para cada variable en un programa, que está disponible en las declaraciones.

En GAP, por otro lado, cada variable solo apunta a un valor, y diferentes variables pueden compartir el mismo valor.

IsBound (para una variable global)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

La función ``IsBound(ident)`` devuelve ``true`` si la variable ``ident`` apunta a un valor y ``false`` en caso contrario.

Para registros y listas, ``IsBound`` se puede usar para verificar si los componentes o las entradas, respectivamente, están vinculados.

.. _Unbind:

Unbind (desvincular un variable)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

La función ``Unbind(ident)`` elimina el identificador ``ident``. Si no hay otra variable que apunte al mismo valor que ``ident``, este valor será eliminado por la siguiente recolección de basura. Por lo tanto, ``Unbind`` se puede utilizar para deshacerse de objetos grandes no deseados.

En el caso de registros y listas, se puede utilizar Unbind_ para eliminar componentes o entradas, respectivamente (ver Capítulos 29 y 21).







