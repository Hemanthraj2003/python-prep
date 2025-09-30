# GENERATORS

## Definition
Generators are a special type of iterator that generate values on-the-fly (lazy evaluation) instead of storing them in memory all at once. They are defined using functions with the `yield` keyword.

## Key Concepts

### 1. Generator Functions
- Functions that contain at least one `yield` statement
- Return a generator object when called
- Execution is paused at `yield` and resumed when `next()` is called

```python
def simple_generator():
    yield 1
    yield 2
    yield 3

gen = simple_generator()
print(next(gen))  # 1
print(next(gen))  # 2
print(next(gen))  # 3
```

### 2. Generator Expressions
- Similar to list comprehensions but use parentheses
- More memory efficient for large datasets

```python
# Generator expression
gen_exp = (x**2 for x in range(5))
print(list(gen_exp))  # [0, 1, 4, 9, 16]

# Compare with list comprehension
list_comp = [x**2 for x in range(5)]  # Creates entire list in memory
```

### 3. Practical Examples

#### Fibonacci Generator
```python
def fibonacci_generator(n):
    a, b = 0, 1
    count = 0
    while count < n:
        yield a
        a, b = b, a + b
        count += 1

# Usage
fib = fibonacci_generator(10)
for num in fib:
    print(num, end=' ')  # 0 1 1 2 3 5 8 13 21 34
```

#### File Reading Generator
```python
def read_large_file(file_path):
    with open(file_path, 'r') as file:
        for line in file:
            yield line.strip()

# Memory efficient for large files
for line in read_large_file('large_file.txt'):
    process_line(line)
```

#### Range Generator (Custom Implementation)
```python
def custom_range(start, end, step=1):
    current = start
    while current < end:
        yield current
        current += step

# Usage
for i in custom_range(0, 10, 2):
    print(i)  # 0 2 4 6 8
```

### 4. Generator Methods

#### send() Method
```python
def generator_with_send():
    value = yield "Ready"
    while True:
        value = yield f"Received: {value}"

gen = generator_with_send()
print(next(gen))  # Ready
print(gen.send("Hello"))  # Received: Hello
print(gen.send("World"))  # Received: World
```

#### throw() and close() Methods
```python
def robust_generator():
    try:
        while True:
            value = yield
            print(f"Processing: {value}")
    except GeneratorExit:
        print("Generator closed")
    except Exception as e:
        print(f"Exception caught: {e}")

gen = robust_generator()
next(gen)
gen.send("data")
gen.throw(ValueError("Test error"))
gen.close()
```

### 5. Advanced Patterns

#### Generator Pipeline
```python
def numbers():
    for i in range(10):
        yield i

def squares(nums):
    for num in nums:
        yield num ** 2

def even_only(nums):
    for num in nums:
        if num % 2 == 0:
            yield num

# Pipeline
pipeline = even_only(squares(numbers()))
print(list(pipeline))  # [0, 4, 16, 36, 64]
```

#### Infinite Generator
```python
def infinite_counter(start=0):
    while True:
        yield start
        start += 1

counter = infinite_counter(10)
for _ in range(5):
    print(next(counter))  # 10, 11, 12, 13, 14
```

## Memory Efficiency Comparison

```python
import sys

# List - stores all values in memory
numbers_list = [x for x in range(1000000)]
print(f"List size: {sys.getsizeof(numbers_list)} bytes")

# Generator - generates values on demand
numbers_gen = (x for x in range(1000000))
print(f"Generator size: {sys.getsizeof(numbers_gen)} bytes")
```

## Interview Insights

### Common Interview Questions

1. **"What's the difference between a generator and a regular function?"**
   - Regular functions return values and terminate
   - Generators yield values and can be resumed
   - Generators maintain state between calls

2. **"When would you use generators over lists?"**
   - Large datasets that don't fit in memory
   - Infinite sequences
   - Processing streams of data
   - When you only need values one at a time

3. **"How do generators save memory?"**
   - Lazy evaluation - values computed on demand
   - Only one value in memory at a time
   - No need to store entire sequence

### Performance Considerations

```python
import time
import memory_profiler

# Memory efficient generator approach
def process_large_dataset_generator():
    for i in range(1000000):
        yield i * 2

# Memory intensive list approach
def process_large_dataset_list():
    return [i * 2 for i in range(1000000)]

# Timing comparison
start = time.time()
gen_result = sum(process_large_dataset_generator())
gen_time = time.time() - start

start = time.time()
list_result = sum(process_large_dataset_list())
list_time = time.time() - start

print(f"Generator time: {gen_time:.4f}s")
print(f"List time: {list_time:.4f}s")
```

### Key Interview Points

1. **State Preservation**: Generators remember where they left off
2. **Lazy Evaluation**: Values computed only when needed
3. **Memory Efficiency**: Constant memory usage regardless of data size
4. **One-time Iteration**: Generators are exhausted after full iteration
5. **Exception Handling**: Can handle exceptions gracefully with try/except

### Common Pitfalls

```python
# Pitfall 1: Generator exhaustion
gen = (x for x in range(3))
print(list(gen))  # [0, 1, 2]
print(list(gen))  # [] - exhausted!

# Pitfall 2: Multiple iterations
def my_generator():
    for i in range(3):
        yield i

gen1 = my_generator()
gen2 = my_generator()  # Need new generator for fresh iteration

# Pitfall 3: Storing generator results
results = []
gen = (x**2 for x in range(1000000))
for value in gen:
    results.append(value)  # Defeats the purpose of generators!
```

## Best Practices

1. Use generators for large datasets or infinite sequences
2. Combine generators for data processing pipelines
3. Handle StopIteration exceptions appropriately
4. Don't store generator results if memory efficiency is the goal
5. Use generator expressions for simple transformations
6. Consider using `itertools` module for advanced generator patterns

## Related Concepts
- Iterators and the Iterator Protocol
- List Comprehensions
- Memory Management
- Lazy Evaluation
- Functional Programming Patterns
