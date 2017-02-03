Language syntax
***************

.. important::

    Since CRSX 4. Previous CRSX uses a different syntax.

Language syntax provides an alternative way to define enumerations using an EBNF notation.
This is very useful in particular when dealing with language constructs.

For instance look at this typical constant declaration EBNF-like grammar definition::

   let : 'let' id '=' LITERAL

where :code:`let` is a grammar rule definition, followed by the grammar rule elements. In CRSX, this is equivalent to the following enumeration::

   Let_Sort ::= ( Let[$String /* 'let' */, Id_Sort, $String /* '=' */, $String /* LITERAL */]; );

While this can shorten enumeration definition, language syntax can also be used in pattern and contraction, for instance::

   Compile[%let⟨ let #id = #LITERAL ⟩] → ...

CRSX automatically generates a *meta parser* for each grammar rule, in this case :code:`let`.
The meta parser statement :code:`%let⟨ code ⟩` indicates *code* follows the let meta syntax.

The following sections semi-formally describe the language syntax feature.

Rules
-----

CRSX allows defining *grammar rule* as follows::

   rulename : block ;

For each grammar rule, CRSX generates an enumeration and a meta parser statement::

   Rulename_Sort ::= ( ... );
   %rulename⟨ ...  ⟩

Alternatives
------------

A grammar rule consists of a set of *alternatives* separated by the '|' character::

   rulename : alternative1 | ... | alternativen ;

Each alternative corresponds to an enumeration value:::

   Rulename_Sort ::= ( Rulename_A1[ ... ]; ... ;  Rulename_A2[ ... ];  );

when the rule contains only one alternative, the *_An* suffix is omitted.

Elements
--------

An alternative is composed of a sequence, possibly empty, of *elements*. An element can either be:

* a literal ;
* a reference to a rule ;
* or a reference to a token.

Sort
^^^^

Literals and tokens are always mapped to the :code:`$String` sort. Rule references are mapped to their corresponding sort name.
Going back to the let example above, here how it looks:::

   let : 'let' id '=' LITERAL
   Let_Sort ::= ( Let[$String /* 'let' literal */, Id_Sort <!-- rule reference -->, $String /* '=' literal */, $String <!-- token reference -->]; );

In the case the alternative has no element, the generated constructor has no arguments.

Pattern
^^^^^^^

For each rule and token declaration, there is a special grammar token used to match any terms. The name of the special token is constructed as follows::

   META_CHAR rulename [0-9]*

By default, *META_CHAR* is the '#' character.::

   Compile[ %let⟨ let #id = #LITERAL ⟩ ] → ...

 says to match any terms for the rule reference :code:`id` and for the token reference :code:`LITERAL`. This pattern is expanded to the following term::

   Compile[ LET[#1, #id, #3, #LITERAL ] ] → ...

*#1* and *#3* both matches literals. They are kept around as they contains location information usefull for some applications, like pretty printers.

Contraction
^^^^^^^^^^^

Each element, other than literals, can be constructed by arbitrary rewrites. This is achieved by *embedding* CRSX code within the language syntax.

For instance::

   Compile[ %let⟨ let #id = #LITERAL ⟩ ] → %local⟨ var ⟨id: #id⟩ := #LITERAL ⟩;
