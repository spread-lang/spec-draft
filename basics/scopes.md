# Block scopes
Any set of instructions could be grouped with `{ }` constructions.
What it does: it encapsulates(black-boxes) this set variables and operations from any outside location.
Meanwhile it feeds external data from parent scope, in a way similar closures does.

*Discuss: if each file treats as a scope, or only program in general does?*

# Scope accessing
Scopes with tiny syntax-sugar could be referred and then reused as functions, classess, promises handlers, etc.
For example:

```
scope { a += 5 }
a = 5
scope
a = 10
scope
```

Code above will init variables closure each time scope is refered.

# Scope interrupt

At any given point scope could be interrupted. If any external variables were changed, program state updates respectively.
Commands for altering execution is `break retval;` and `continue retval;`. If no ret value specified, it means no return value from scope were
provided.
