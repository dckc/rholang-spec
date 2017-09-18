***********************
Language Specification
***********************

Syntax
-------

First we present the basic syntax.

**RChain’s Social Contract Language**

RChain offers a typed contract language that provides a semantics
naturally suited for decentralized, distributed computing. It is built
around communicating mobile processes.

Top level declarations and control flow
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. productionlist:: syntax-top
      contract : "contract" `name` "(" `ptrn` [":" `sort`] [, `ptrn` [":" `sort`]]* ")" [":" `type`] "=" "{" `process` "}"
      process  : "Nil"
               : "for" "(" `ptrn` "<-" `channel` [";" `ptrn` <- `channel` ]* [";" "if" `cond`] ")" `process`
               : "select" "{" `branch` [";" `branch` ]* "}"
               : "match" `process` "{" `ptrn` "=>" `process` [";" `ptrn` "=>" `process` ]* "}"
               : `channel` "!" "(" `process` ["," `process` ]* ")"
               : `process` "|" `process`
               : "*" `channel`
               : "new" `var` ["," `var` ]* "in" `process`
               : `name` "(" `process` ["," `process` ]* ")"
               : `value`
      branch   : "case" `ptrn` "<"- `channel` [";" `ptrn` "<-" `channel` ]* "=>" `process`

Builtin types
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. productionlist:: syntax-builtin-types
    channel    : `address`
               : `var`
    address    : `keccak256`
               : "@" `process`
    value      : `quantity`
               : `entity`
    quantity   : `boolean`
               : `integer`
    entity     : `char`
               : `datetime`
               : `struct`
               : `collection`
    struct     : `name` "{" `process` ["," `process` ]* "}"
    collection : `string`
               : `array`
               : `list`

Pattern matching
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. productionlist:: syntax-matching
         ptrn  : "Nil"
               : "for" "(" `var` [";" "if" `var`] ")" `ptrn`
               : `channel` "!" "(" `ptrn` ["," `ptrn` ]* ")"
               : `ptrn` "|" `ptrn`
               : "*" `channel`
               : `name` "(" `ptrn` ["," `ptrn`]* ")"
               : `vptrn`
               : "match" `ptrn` "{" `var` "}"
         vptrn : `strptrn`
               : `aptrn`
               : `lptrn`

Behavioral types
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. productionlist:: syntax-behavioral-types

       property : "property" `name` "(" `var` ["," `var` ]* ")" "=" `type`
          type  : "true"
                : "Void"
                : "~" `type`
                : `type` "&" `type`
                : `type` "|" `type`
                : "hidden" `var` ["," `var` ]* "." `type`
                : `channel` "?" `type`
                : `channel` "!" "(" `type` ["," `type` ]* ")"
                : `name` "(" `channel` ["," `var` ]* ")"
