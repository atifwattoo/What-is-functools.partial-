### **What is `functools.partial` in Python?**
`functools.partial` creates a new function by partially applying arguments to an existing function. This is helpful for simplifying function calls in scenarios where certain arguments are repetitive or fixed.

The `functools.partial` function in Python allows you to "freeze" some portion of a function's arguments or keywords, creating a new function with fewer parameters. It's especially useful when you want to fix certain parameters of a function while keeping others flexible.

```python
from functools import partial
```

---

### **Basic Syntax**
```python
partial(func, *args, **kwargs)
```

- **`func`**: The function to partially apply.
- **`*args`**: Positional arguments to fix.
- **`**kwargs`**: Keyword arguments to fix.

The returned object is a new function where the fixed arguments are "frozen," and you only need to supply the remaining ones when calling the new function.

---

### **Examples**

#### **1. Partially Fixing Arguments**
```python
def power(base, exponent):
    return base ** exponent

# Create a square function by fixing exponent = 2
square = partial(power, exponent=2)

# Now, square() only needs the base
print(square(5))  # Output: 25
print(square(10))  # Output: 100
```

Here, `partial` creates a new function `square` that always uses `exponent=2`.

---

#### **2. Simplifying Function Calls**
Suppose you have a function with multiple arguments, and you often call it with some fixed values.

```python
def greet(greeting, name):
    return f"{greeting}, {name}!"

# Fix the greeting
say_hello = partial(greet, greeting="Hello")
say_goodbye = partial(greet, greeting="Goodbye")

print(say_hello("Alice"))   # Output: Hello, Alice!
print(say_goodbye("Alice")) # Output: Goodbye, Alice!
```

---

#### **3. Partial for Use in Mapping**
You can use `partial` to adapt a function for operations like `map`.

```python
def multiply(x, y):
    return x * y

# Fix y = 10
multiply_by_10 = partial(multiply, y=10)

# Use in a map
numbers = [1, 2, 3, 4]
result = map(multiply_by_10, numbers)
print(list(result))  # Output: [10, 20, 30, 40]
```

---

#### **4. Partial with Functions That Have Default Arguments**
Partial works seamlessly with functions that already have default arguments.

```python
def add(a, b=10):
    return a + b

# Fix b to 20
add_with_20 = partial(add, b=20)
print(add_with_20(5))  # Output: 25
```

---

#### **5. Combining with Other Libraries (e.g., Pandas or JSON)**
You can use `partial` with libraries like **Pandas** to simplify repetitive operations.

```python
import pandas as pd

def filter_rows(df, column, value):
    return df[df[column] == value]

# Fix the column name
filter_by_age = partial(filter_rows, column="age")

# Example DataFrame
data = pd.DataFrame({"name": ["Alice", "Bob"], "age": [25, 30]})
result = filter_by_age(data, value=25)
print(result)
```

---

### **When to Use `functools.partial`**
1. **Reusable Logic**:
   - When you want to create reusable versions of a function with fixed arguments.
2. **Simplifying Callbacks**:
   - Useful for libraries like `tkinter`, `asyncio`, or `threading`, where callbacks often require simpler signatures.
3. **Functional Programming**:
   - Works well with `map`, `filter`, or similar operations.
4. **Improving Readability**:
   - Makes code cleaner by reducing redundant arguments.

---

### **Notes and Best Practices**
- **Inspecting Partial Functions**:
   You can inspect the frozen arguments of a partial function using `partial.func`, `partial.args`, and `partial.keywords`.

   ```python
   print(square.func)       # Original function (power)
   print(square.keywords)   # {'exponent': 2}
   print(square.args)       # ()
   ```

- **Flexibility**:
   You can override frozen arguments when calling the partial function.
   ```python
   print(square(5, exponent=3))  # Output: 125 (exponent is overridden)
   ```

---

### **Advanced Example: Using Partial for Higher-Order Functions**

```python
def apply_discount(price, discount):
    return price - (price * discount)

# Fix a 10% discount
discount_10 = partial(apply_discount, discount=0.10)

prices = [100, 200, 300]
discounted_prices = map(discount_10, prices)
print(list(discounted_prices))  # Output: [90.0, 180.0, 270.0]
```

---

Using `functools.partial` can simplify and clean up your code, especially when dealing with repetitive function calls or higher-order functions. Let me know if you'd like more examples or advanced use cases!
