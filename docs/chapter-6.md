# Chapter 6: Functions and Abstraction

In Chapter 5, conditions allowed a program to choose a path.

Now we ask:

> Can I give a name to a process and use that process again?

A function is a named process. It may receive values, perform actions, and return a result.

```python
def double(number):
    return number * 2
```

The function does not run merely because it has been defined. It runs when it is called.

---

## Part 6.1 - Defining and Calling a Function

| Question | Answer |
| :--- | :--- |
| True or false: defining a function immediately runs its body.<br><br>`def greet():`<br>&emsp;`print('Hello')` | **False.** Defining the function gives the process a name, but the body has not run yet. |
| What happens when this line runs?<br><br>`greet()` | The function body runs and prints `Hello`. |
| True or false: `greet` and `greet()` mean the same thing. | **False.** `greet` refers to the function. `greet()` calls the function. |
| What does the word `def` begin? | A function definition. |
| What does the indentation after `def` identify? | The statements belonging to the function body. |
| How many times does the body run?<br><br>`greet()`<br>`greet()`<br>`greet()` | Three times. Each call runs the body once. |
| What happens if a function is defined but never called? | Its body never runs. |

### Fundamental Questions About Definition and Calling

| Question | Answer |
| :--- | :--- |
| What does defining a function do? | It gives a process a name and stores its instructions. |
| What does calling a function do? | It begins executing the stored instructions. |
| Can a function be called more than once? | Yes. Each call begins a new execution of the function body. |
| Why are functions useful? | They let us name, reuse, test, and compose processes. |

> ### Law of Definition and Calling
> Defining a function only names and stores a process; nothing runs until the function is called.

---

## Part 6.2 - Parameters and Arguments

A parameter is a name inside the function definition. An argument is a value supplied when the function is called.

```python
def double(number):
    return number * 2

double(7)
```

| Question | Answer |
| :--- | :--- |
| What is `number` in `def double(number):`? | A parameter. It is the name the function uses for its incoming value. |
| What is `7` in `double(7)`? | An argument. It is the value supplied to the parameter. |
| What is returned by `double(7)`? | `14`. |
| What is returned by `double(-3)`? | `-6`. |
| What is returned by `double(0)`? | `0`. |
| Can the parameter and argument have the same data type? | Yes. They often do, but Python does not require a declared type in the function definition. |
| What happens when `double()` is called without an argument? | Python raises a `TypeError` because the required parameter has no value. |
| What happens when `double(2, 3)` is called with two arguments? | Python raises a `TypeError` because the function accepts only one argument. |

### Fundamental Questions About Parameters

| Question | Answer |
| :--- | :--- |
| What does a parameter represent? | A temporary name for a value the function will receive. |
| What does an argument represent? | The actual value supplied to a parameter during a call. |
| Can one function call use different arguments? | Yes. `double(4)` and `double(10)` use the same process with different values. |
| Does changing a parameter name change the function's purpose? | No. The name may change, but the parameter should describe the incoming value. |

---

## Part 6.3 - Returning a Value

A function can print something, but printing is not the same as returning.

```python
def double(number):
    return number * 2
```

| Question | Answer |
| :--- | :--- |
| What does `return` do? | It sends a value back to the place where the function was called. |
| What is the value of `result`?<br><br>`result = double(5)` | `10`. |
| Can the returned value be assigned to a variable? | Yes. |
| Can the returned value be used in another expression?<br><br>`score = double(5) + 3` | Yes. `score` becomes `13`. |
| What happens after Python executes a `return` statement? | The function ends immediately and sends the returned value back. |
| What is the value of `result`?<br><br>`def announce():`<br>&emsp;`print('Ready')`<br><br>`result = announce()` | `None`. The function printed a message but did not return a value. |
| What appears on the screen when `announce()` runs? | `Ready`. Printing creates visible output; it does not create a return value. |

### Fundamental Questions About Print and Return

| Question | Answer |
| :--- | :--- |
| What does `print` do? | It displays a value. |
| What does `return` do? | It provides a value to the calling code. |
| Can a function print and return? | Yes. These are separate actions. |
| What does a function return if it reaches the end without `return`? | `None`. |
| Which is more reusable: a function that only prints a result or one that returns it? | A function that returns it. The caller can print, store, compare, or transform the returned value. |

> ### Law of Print and Return
> `print` displays a value for a person; `return` hands a value back to the calling code — a function that returns is reusable in ways a function that only prints is not.

---

## Part 6.4 - Functions as Named Rules

A useful function gives a name to a rule or process.

| Question | Answer |
| :--- | :--- |
| What does this function decide?<br><br>`def is_alive(health):`<br>&emsp;`return health > 0` | Whether `health` is greater than zero. |
| What is `is_alive(10)`? | `True`. |
| What is `is_alive(0)`? | `False`. |
| What is `is_alive(-4)`? | `False`. |
| What does this function decide?<br><br>`def has_key(inventory):`<br>&emsp;`return 'key' in inventory` | Whether `'key'` is in the inventory. |
| What is `has_key(['sword', 'key'])`? | `True`. |
| What is `has_key(['sword', 'potion'])`? | `False`. |
| Why is `is_alive(health)` clearer than repeating `health > 0` throughout a program? | The name communicates the concept, while the implementation remains in one place. |

### Fundamental Questions About Named Processes

| Question | Answer |
| :--- | :--- |
| What is abstraction? | Giving a meaningful name to a process so we can use it without rewriting its internal steps. |
| What should a function name communicate? | What the function does or what question it answers. |
| Is a predicate a good candidate for a function? | Yes. Functions such as `is_alive` and `has_key` can name Boolean rules. |
| Does naming a process remove the process? | No. It gives the process a reusable boundary. |

---

## Part 6.5 - Functions Can Use Conditions

A function can contain the same decisions students already know, but the decision is now reusable.

| Question | Answer |
| :--- | :--- |
| What is returned by this function?<br><br>`def status(health):`<br>&emsp;`if health > 0:`<br>&emsp;&emsp;`return 'alive'`<br>&emsp;`else:`<br>&emsp;&emsp;`return 'defeated'` | It returns `'alive'` when `health > 0`; otherwise it returns `'defeated'`. |
| What is `status(10)`? | `'alive'`. |
| What is `status(0)`? | `'defeated'`. |
| What is returned by this function?<br><br>`def classify(score):`<br>&emsp;`if score >= 90:`<br>&emsp;&emsp;`return 'gold'`<br>&emsp;`elif score >= 50:`<br>&emsp;&emsp;`return 'silver'`<br>&emsp;`else:`<br>&emsp;&emsp;`return 'bronze'` | One of the three strings, depending on the score. |
| What is `classify(95)`? | `'gold'`. |
| What is `classify(75)`? | `'silver'`. |
| What is `classify(20)`? | `'bronze'`. |
| Can a function return a Boolean expression directly?<br><br>`def is_even(number):`<br>&emsp;`return number % 2 == 0` | Yes. The function returns `True` or `False`. |

---

## Part 6.6 - Functions Can Be Composed

The returned value from one function can become the argument to another.

```python
def double(number):
    return number * 2

def add_one(number):
    return number + 1
```

| Question | Answer |
| :--- | :--- |
| What is `add_one(double(3))`? | `7`. `double(3)` produces `6`, then `add_one(6)` produces `7`. |
| Which function runs first in `add_one(double(3))`? | `double`, because its result is needed before `add_one` can run. |
| What is `double(add_one(3))`? | `8`. `add_one(3)` produces `4`, then `double(4)` produces `8`. |
| What is the value of `result`?<br><br>`result = is_alive(status_health)` | The Boolean result returned by `is_alive` for `status_health`. |
| Can a function call appear inside a conditional?<br><br>`if is_alive(health):` | Yes. The returned Boolean controls the branch. |
| Can a function call appear inside a comprehension?<br><br>`[double(x) for x in range(4)]` | Yes. The function transforms each source value. |
| What is `[double(x) for x in range(4)]`? | `[0, 2, 4, 6]`. |

### Fundamental Questions About Composition

| Question | Answer |
| :--- | :--- |
| What does composition mean? | Combining smaller functions so that one function's result becomes another function's input. |
| Why is composition useful? | Each function can do one clear job while larger behavior emerges from their combination. |
| What should a function return if it is meant to be composed? | A useful value rather than only printed text. |
| What is the danger of a function that both changes the world and returns an unclear value? | It becomes harder to predict, test, and reuse. |

> ### Law of Function Effects
> Ask three questions of any function: what values does it receive, what value does it return, and what objects does it change?

---

## Part 6.7 - Default Parameters

A parameter can have a default value.

```python
def greet(name='traveler'):
    return 'Hello, ' + name
```

| Question | Answer |
| :--- | :--- |
| What is `greet('Ada')`? | `'Hello, Ada'`. The supplied argument replaces the default. |
| What is `greet()`? | `'Hello, traveler'`. No argument was supplied, so the default is used. |
| What does a default parameter provide? | A fallback value when the caller does not provide an argument. |
| What is `def repeat(value, count=2):` saying? | `value` is required; `count` is optional and defaults to `2`. |
| What is `repeat('go')` if the function returns `[value for _ in range(count)]`? | `['go', 'go']`. |
| What is `repeat('go', 3)`? | `['go', 'go', 'go']`. |
| Can a required parameter come after a default parameter? | No. Required parameters must come before parameters with defaults. |

### Fundamental Questions About Defaults

| Question | Answer |
| :--- | :--- |
| Does a default value run every time the function is called? | It is used only when the caller does not provide that argument. |
| Can the caller override a default? | Yes. |
| Why are defaults useful? | They allow a function to have a simple common use while still supporting customization. |

---

## Part 6.8 - Keyword Arguments

Arguments may be supplied by position or by parameter name.

```python
def move(x, y, speed):
    return (x + speed, y)
```

| Question | Answer |
| :--- | :--- |
| What is `move(2, 3, 1)`? | `(3, 3)`. The arguments match `x`, `y`, and `speed` by position. |
| What is `move(x=2, y=3, speed=1)`? | `(3, 3)`. The arguments match by keyword. |
| What is `move(speed=1, y=3, x=2)`? | `(3, 3)`. Keyword arguments do not need to appear in parameter order. |
| What is the advantage of a keyword argument? | The name documents which parameter is receiving the value. |
| Can positional and keyword arguments be combined? | Yes, but positional arguments must come first. |
| What is invalid about `move(x=2, 3, speed=1)`? | A positional argument appears after a keyword argument. |

---

## Part 6.9 - Local Names and External State

Parameters and variables created inside a function are normally local to that function.

| Question | Answer |
| :--- | :--- |
| What is `result`?<br><br>`def double(number):`<br>&emsp;`result = number * 2`<br>&emsp;`return result` | The local variable `result` is used inside the function and its value is returned. |
| Can code outside the function directly use the local variable `result` after the call? | No. The local name does not exist outside the function. |
| What is `x` after this code?<br><br>`x = 10`<br>`def change():`<br>&emsp;`x = 5`<br>`change()` | `10`. The function creates a local `x`; it does not replace the outer `x`. |
| How can the function change the outer value of `x` safely? | Return a new value and assign it: `x = change(x)`. |
| Why is returning a value often clearer than changing a global variable? | The function's inputs and outputs are visible at the call site. |

### Fundamental Questions About Local Work

| Question | Answer |
| :--- | :--- |
| What is a local variable? | A name created inside a function that belongs to that function's execution. |
| What happens to local variables when the function returns? | Their local scope ends. |
| Can a function read a list passed as an argument? | Yes. |
| Can a function mutate a list passed as an argument? | Yes. A list is mutable, so methods such as `.append()` can change the same list object. |
| Is mutating an argument the same as returning a replacement? | No. Mutation changes an existing object. Returning provides a value to the caller. |

---

## Part 6.10 - Returning Versus Mutating

These two functions may appear similar, but they communicate differently.

```python
def doubled(numbers):
    return [number * 2 for number in numbers]

def double_in_place(numbers):
    for i in range(len(numbers)):
        numbers[i] = numbers[i] * 2
```

| Question | Answer |
| :--- | :--- |
| Does `doubled(numbers)` change the original list? | No. It returns a new list. |
| Does `double_in_place(numbers)` change the original list? | Yes. It mutates the list through its indices. |
| What is the value of `result`?<br><br>`numbers = [1, 2, 3]`<br>`result = doubled(numbers)` | `result` is `[2, 4, 6]`; `numbers` remains `[1, 2, 3]`. |
| What is the value of `numbers`?<br><br>`numbers = [1, 2, 3]`<br>`double_in_place(numbers)` | `[2, 4, 6]`. |
| What does `double_in_place(numbers)` return if it has no `return` statement? | `None`. Its important effect is the mutation of `numbers`. |
| Which function is easier to reason about as a transformation? | `doubled`, because its result is returned and the input remains unchanged. |
| Is mutation always wrong? | No. Mutation is useful when a function is intentionally responsible for changing shared state. |

### Fundamental Questions About Function Effects

| Question | Answer |
| :--- | :--- |
| What should you ask when reading a function? | What values does it receive, what value does it return, and what objects does it change? |
| What is a pure transformation? | A function that returns a result without changing external state. |
| What is a side effect? | A visible change outside the function's local calculation, such as printing, appending, or changing a shared object. |
| Why should a function's side effects be intentional? | Hidden changes make a program harder to predict and compose. |

---

## Part 6.11 - Functions as Values

Functions can themselves be stored in variables and passed to other functions.

| Question | Answer |
| :--- | :--- |
| What is `operation` after `operation = double`? | A reference to the function `double`. The function has not been called. |
| What is `operation(5)` when `operation = double`? | `10`. The stored function is called with `5`. |
| What is the difference between `operation = double` and `operation = double(5)`? | The first stores the function. The second calls it and stores its result, `10`. |
| Can a function receive another function as an argument? | Yes. |
| What should the receiving function do with that argument? | It can call the supplied function. |
| What does `apply(double, 4)` return if `apply` is defined as `return operation(value)`? | `8`. |
| Is this the same idea as a lambda function? | It is related. A named function can be passed as a value without introducing lambda syntax yet. |

---

## Part 6.12 - Predict the Result

Do not run these immediately. Predict the final value and the data type.

| Question | Answer |
| :--- | :--- |
| What is `result`?<br><br>`def triple(number):`<br>&emsp;`return number * 3`<br><br>`result = triple(4)` | `12`, an integer. |
| What is `result`?<br><br>`def announce():`<br>&emsp;`print('Ready')`<br><br>`result = announce()` | `None`. The function prints but does not return a value. |
| What is `result`?<br><br>`def add_one(number):`<br>&emsp;`return number + 1`<br><br>`result = add_one(add_one(3))` | `5`, an integer. |
| What is `state`?<br><br>`def state_for(health):`<br>&emsp;`return 'alive' if health > 0 else 'defeated'`<br><br>`state = state_for(0)` | `'defeated'`, a string. |
| What is `result`?<br><br>`def is_even(number):`<br>&emsp;`return number % 2 == 0`<br><br>`result = [number for number in range(5) if is_even(number)]` | `[0, 2, 4]`, a list. |
| What is `numbers`?<br><br>`def double_in_place(numbers):`<br>&emsp;`for i in range(len(numbers)):`<br>&emsp;&emsp;`numbers[i] *= 2`<br><br>`numbers = [1, 2, 3]`<br>`result = double_in_place(numbers)` | `numbers` is `[2, 4, 6]`; `result` is `None`. |
| What is `result`?<br><br>`def repeat(value, count=2):`<br>&emsp;`return [value for _ in range(count)]`<br><br>`result = repeat('go', 3)` | `['go', 'go', 'go']`, a list. |
| What is `result`?<br><br>`def apply(operation, value):`<br>&emsp;`return operation(value)`<br><br>`def square(number):`<br>&emsp;`return number ** 2`<br><br>`result = apply(square, 5)` | `25`, an integer. |

---

## Chapter Summary

| Question | Answer |
| :--- | :--- |
| What does defining a function do? | It names and stores a process. |
| What does calling a function do? | It executes the process. |
| What is a parameter? | A name for an incoming value inside a function definition. |
| What is an argument? | The actual value supplied during a function call. |
| What does `return` do? | It sends a value back to the caller and ends the function. |
| What does a function return without a `return` statement? | `None`. |
| What is abstraction? | Naming a process so it can be reused without rewriting its internal steps. |
| What is composition? | Using the result of one function as the input to another. |
| What is a default parameter? | A fallback argument used when the caller supplies no value. |
| What is a side effect? | A change outside the function's local calculation, such as printing or mutating an object. |
| What is the central idea? | A function turns a repeated or meaningful process into a named tool that can be called, combined, and reasoned about. |

---

**==> A function gives a name to a path through thought, and lets you walk that path again. <==**
