
# Embellishing

## Why

When you need to add functionality to different types of data that also need to be
perpetual, carrying beyond the original value. Then embellishing a type with
new functionality could be a good way to introduce new functionality and new
abstractions.

## How

### Functors as containers

We will use a simplified model of Functors where they act as a container, a
container with extra information.


Excusing the verbose type annotation in python you get this:

``` python
from __future__ import annotations
from typing import TypeVar, Generic

F = TypeVar('F')
T = TypeVar('T')
FunctorType = TypeVar('FunctorType', bound='Functor')

class Functor:
    """
    >>> functor.map(str)
    '42'
    """
    def map(self: FunctorType[F], f: Callable[[F], T]) -> FunctorType[T]:
        pass
```

A container that can take any Callable that takes one argument of the contained
type and then returns a new value. This lets the Functor preserve structure and
lets you reuse code that don't know about your Functor.

### Applicatives, more power

Applicative is used when you not only have Embellished values but Embellished
Callables. Which most often you have because you want to use functions that
takes more then one argument. It may also make code look nicer since it
naturally move the called value to the left of its arguments.
 
``` python
from __future__ import annotations
from typing import TypeVar, Generic

F = TypeVar('F')
T = TypeVar('T')
AppType = TypeVar('AppType', bound='Applicative')

class Applicative:
    """
    >>> applicative.ap("Hello").ap("World")
    'Hello World'
    """
    def ap(self: AppType[Callable[[F],T]], from: AppType[F]) -> AppType[T]:
        pass
```

In the code above T might it self be a Callable and therefore we may chain
calls to ap to apply arguments to the result.

Bonus points to write a more pythonic version using `*` on the from argument to
chain calls on none-curried functions.
