* Read first: scopes.md *

# Explaining function syntax
Function declaration syntax is the same executable code as any other parts. It means that any developer could built similar
mechanism with language itself.

```
function_name(arguments)
{
  scope
}

function_name(1,2,3)
```

In a nut shell it means that
```
function_object.arguments = arguments;
function_object.scope = scope;

function_registry += function_object

// find function_name in registry, try to closure any required data, and interpret the block scope
```

# What difference between closure and arguments
With closure, program trying to interpret scope at the time and place it were executed.
It uses any outside variables as references, and
altering them:
```
{
  a = 5;
  {
    a += 5;
  }
}
```

This is how block scopes works major of languages, and this is expected. It also clear behaviour with inline functions (lambdas for example).
But for most cases, developer expects complete encapsulation, with data cloning. Function supposed to be black box, which gets some data and
return some data. With no other side effects (not exactly, but lets keep it simple).

That could be achieved with capture groups and arguments.


# Capture group

Capture group `( )`, is sugar syntax for taking source code current state, including variables state, and clone it.
It uses closure technic to resolve each variable name, and then recreate them with same names.

For example
```
a = 1
b = 1
(a)
{
  a += 5;
  b += 5;
}
// a == 1, b == 6
```

# Built-in functions

Probably the most easiest function to explain is `if`. (For simplicity - without `else`).

```
if (arguments) ->
{
  arguments || scope  
}
```

Which in formal language means - try to extract value from scope if arguments are true.
Special operator `->` means that next scope is used to control callbacks, if any were specified.
Any function can have callbacks, if control scope were declared
Function with control scope called control group or control statement.

Other built in control statement such as `for` `while`, etc, utilizes same syntax.

# Explaining code capture

Cycling and other operations in other languages suppose that code in capture group actually going to be executed:

```
a = 5
i = 0;
while (i++ < 10)
{
  a += i;
}
```

From above you might expect that function `for` will receive argument: `true`, and scope will executed once.
Which is not exactly correct. Capture group taking source code, with frozen variables states, not variables itself.
However there is difference. Since capture group don't change outside value, `i` variable still going to be zero, after loop. (But will increment inside loop scope).

So while function simple to declare too:

```
while (cond) ->
{
  flag = cond

  // if condition is false, interrupt execution
  !flag || break
  // if condition is true, execute scope block
  flag || scope(cond)
  // if condition is true, recursive execute remaining iterations
  flag || control(cond)
}
```

Although language utilizes pure parallelism there are way to make cycles single threaded.
(Code above will execute scope and control in parallel, which might be confused at the first time)

Single threaded cycling:
```
while_st (cond) ->
{
  flag = cond

  !flag || break
  , // join statements in single thread pipe
  scope (cond)
  ,
  control (cond)
}
```
