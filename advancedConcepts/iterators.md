# Iterators and Iterables

## Iterator

- An `Iterator` is an object that contains a countable number of values.

- Correct definition:
  An Iterator is any object that implements the iterator protocol, which consists of the methods:

  `__iter__()` → returns the iterator object itself

  `__next__()` → returns the next value or raises StopIteration

- `Note`:

  The “countable number of values” description is just a common use case, not the formal definition.

  Iterators can produce _finite_ or _infinite_ sequences, or even _meaningless/dummy_ values, as long as they implement the protocol.

## Iterable

- `Iterables` implement the `__iter__()` method, which returns an iterator.

- An `Iterable` is an object that can return an `iterator` object when passed to the `iter()` function.
  \
  \
  `NOTE`: an object can be both an `Iterable` and an `Iterator` if it implements both `__iter__()` and `__next__()` methods.
