
#################
Welcome to Tosca!
#################

.. toctree::
   :maxdepth: 2

   started
   intro
   lexical

The purpose of Tosca is to implement an extended higher-order rewriting formalism to facilitate writing compilers
and other syntax-directed transformation systems, specifically:

+ Special notations for using embedded syntax, even higher order abstract syntax.
+ Special support for symbol tables, environments, and attributes, as used in compilers.
+ A polymorphic sort system (which in practice means that Tosca systems are contraction schemes).
+ Tosca systems can be compiled directly to native code (so far in C++ or Java) for effective execution.
