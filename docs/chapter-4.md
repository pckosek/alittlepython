# Chapter 4: Repetition and Process

In Chapter 3, we used comprehensions to describe the contents of a new collection.

```python
[x * 2 for x in range(5)]
```

Now we will slow that idea down.

> What is Python actually doing, one step at a time?

A `for` loop visits values one at a time and performs an action for each value.

```python
for item in items:
    print(item)
```

The sequence supplies the values. The loop variable names the current value. The indented body runs once for each value.

---

## Part 4.1 - Visiting Every Element

| Question | Answer |
| :--- | :--- |
| How many times does the body of this loop run?<br><br>`items = ['sword', 'shield', 'potion']`<br>`for item in items:`<br>&emsp;`print(item)` | Three times. The list contains three elements. |
| What is `item` during the first pass? | `'sword'`. |
| What is `item` during the second pass? | `'shield'`. |
| What is `item` during the third pass? | `'potion'`. |
| In what order are the values printed? | `'sword'`, then `'shield'`, then `'potion'`. The loop follows the order of the list. |
| What happens after the body runs for `'potion'`? | The loop has no more values to visit and ends. |
| What happens when the sequence is empty?<br><br>`for item in []:`<br>&emsp;`print(item)` | The body runs zero times. Nothing is printed. |

### Fundamental Questions About Visiting

| Question | Answer |
| :--- | :--- |
| What does the sequence after `in` provide? | The values that the loop will visit. |
| What does the loop variable represent? | The current value being visited. |
| How many times does the body run? | Once for every value in the sequence. |
| Does a `for` loop automatically create a new list? | No. A loop performs an action. It creates or changes data only if the body tells it to do so. |

> ### Law of Visiting
> A `for` loop runs its body exactly once for every value the sequence supplies — no more, no fewer.

---

## Part 4.2 - The Loop Variable

The loop variable is a temporary name. It takes on a different value during each pass.

| Question | Answer |
| :--- | :--- |
| What is printed?<br><br>`for word in ['cat', 'horse', 'owl']:`<br>&emsp;`print(word)` | `cat`, then `horse`, then `owl`. |
| Is `word` an index? | No. It is the current value from the list. |
| What is printed?<br><br>`for number in [10, 20, 30]:`<br>&emsp;`print(number)` | `10`, then `20`, then `30`. |
| Could the loop variable be called `item` instead of `number`? | Yes. The name is chosen by the programmer. |
| Could the loop variable be called `banana`? | Yes, although a descriptive name such as `number` is clearer. |
| What is the value of `item` after this loop?<br><br>`for item in ['a', 'b', 'c']:`<br>&emsp;`pass` | `'c'`. The loop variable retains the last value it visited. |
| What happens if the sequence is empty and we ask for `item` afterward? | `item` was never assigned by the loop, so Python raises a `NameError`. |

### Fundamental Questions About the Current Value

| Question | Answer |
| :--- | :--- |
| Does the loop variable permanently represent one object? | No. It is rebound to the next value on every pass. |
| Does changing the name of the loop variable change the loop’s behavior? | No. The name changes, but the sequence and body determine the behavior. |
| What should a good loop variable name communicate? | What kind of value is currently being visited. |

---

## Part 4.3 - Repeating an Action

A loop can perform an action for every value without creating a new collection.

| Question | Answer |
| :--- | :--- |
| What is printed?<br><br>`items = ['sword', 'shield']`<br>`for item in items:`<br>&emsp;`print('Collected:', item)` | `Collected: sword`, then `Collected: shield`. |
| What is printed?<br><br>`for name in ['Ada', 'Grace', 'Alan']:`<br>&emsp;`print('Hello', name)` | `Hello Ada`, then `Hello Grace`, then `Hello Alan`. |
| How many times does `print('Jump')` run?<br><br>`for _ in range(4):`<br>&emsp;`print('Jump')` | Four times. The loop variable is not needed, so `_` is used. |
| Does `print` change the list being visited? | No. It displays information but does not modify the list. |
| What happens if the body contains two statements? | Both statements run once for every value in the sequence. |

### Fundamental Questions About the Body

| Question | Answer |
| :--- | :--- |
| Which lines belong to the loop body? | The indented lines immediately following the `for` statement. |
| What happens if a statement is not indented? | It runs after the loop, not during every pass. |
| What is the purpose of indentation in a `for` loop? | Indentation tells Python which statements should repeat. |

---

## Part 4.4 - Accumulating a Result

A loop can carry information from one pass into the next.

```python
score = 0

for value in [10, 20, 5]:
    score = score + value
```

| Question | Answer |
| :--- | :--- |
| What is the initial value of `score`? | `0`. |
| What is `score` after the first pass? | `10`. |
| What is `score` after the second pass? | `30`. |
| What is `score` after the third pass? | `35`. |
| What does `score = score + value` do? | It uses the old score and the current value to create a new score. |
| What survives from one pass to the next? | The current value of `score`. |
| What is the final value of `score` if the list is empty? | `0`. The loop body never runs, so the initial value remains. |

### Fundamental Questions About Accumulation

| Question | Answer |
| :--- | :--- |
| What is an accumulator? | A variable that carries a partial result from one pass into the next. |
| Where should the accumulator usually be created? | Before the loop begins. |
| What happens if the accumulator is reset inside the loop?<br><br>`for value in values:`<br>&emsp;`score = 0`<br>&emsp;`score = score + value` | The previous result is lost on every pass. The loop does not accumulate across iterations. |
| What is the difference between `score = score + value` and `score = value`? | The first preserves the previous score and adds to it. The second replaces the previous score. |

> ### Law of Accumulation
> An accumulator must be created before the loop and updated, never reset, inside it — otherwise nothing survives from one pass to the next.

---

## Part 4.5 - Building a New List with a Loop

In Chapter 3, a comprehension built a new list compactly.

```python
[x * 2 for x in range(5)]
```

The same process can be written as a loop:

```python
result = []

for x in range(5):
    result.append(x * 2)
```

| Question | Answer |
| :--- | :--- |
| What is the initial value of `result`? | An empty list. |
| What happens during each pass? | `x * 2` is calculated and appended to `result`. |
| What is the final value of `result`? | `[0, 2, 4, 6, 8]`. |
| How many times does `result.append(...)` run? | Five times. `range(5)` provides five values. |
| What happens if `result = []` is placed inside the loop? | The list is recreated on every pass, so earlier values are lost. |
| Which comprehension is equivalent to the loop?<br><br>`result = []`<br>`for x in range(5):`<br>&emsp;`result.append(x * 2)` | `[x * 2 for x in range(5)]`. |

### Fundamental Questions About Collection Building

| Question | Answer |
| :--- | :--- |
| What three ingredients build a list with a loop? | An empty result list, a repeated action, and `append`. |
| What does `append` do in this pattern? | It preserves each newly calculated value by adding it to the result list. |
| What is the relationship between this loop and a comprehension? | The loop is the expanded process. The comprehension is the compact version of the same common pattern. |

> ### Law of Collection Building
> Build a new list with a loop by starting from `[]`, then `append`-ing one computed value per pass; a comprehension is the same pattern written compactly.

---

## Part 4.6 - Growing a List vs. Filling a List

`result.append(...)` worked for every list built so far. It is not the only way to build one.

| Question | Answer |
| :--- | :--- |
| Does `append` do work every time it is called? | Yes. It finds the end of the list and adds one element there. |
| Is that the same small amount of work no matter how large the list eventually becomes? | Not quite. A list occasionally has to grow its underlying storage as it fills up, which adds a little extra cost on top of an ordinary append. For a handful of values this is unnoticeable. For a grid with thousands of cells, appending one value at a time is doing more work than it needs to. |
| If the final length is already known before the loop starts, is there a more direct way to build the list? | Yes. Create a list at that length first, filled with a placeholder value, and then assign into each position by index instead of appending. |

```python
result = [0] * 5

for i in range(5):
    result[i] = i * 2
```

| Question | Answer |
| :--- | :--- |
| What does `[0] * 5` create? | A list of five placeholder zeros: `[0, 0, 0, 0, 0]`. |
| What does `result[i] = i * 2` do that `result.append(i * 2)` does not? | It writes directly into a slot that already exists, instead of growing the list to make room for a new one. |
| Is `result` still `[0, 2, 4, 6, 8]` at the end? | Yes. Both versions produce the same final list. |
| When would this matter for a grid? | A grid's final size — `width * height` — is known before a single cell is generated. Pre-sizing the list and filling it by index avoids repeatedly growing a large list one cell at a time. |
| When is `append` still the right choice? | When the final size is not known in advance — for example, when values are kept only if they pass a filter, so the count of survivors cannot be predicted before the loop runs. |

### Fundamental Questions About Growing vs. Filling

| Question | Answer |
| :--- | :--- |
| What two ingredients does the filled-list version need before the loop? | A list already at its final length, and a placeholder value to fill it with. |
| What is the risk of choosing the wrong placeholder? | A bug where an untouched slot is mistaken for a computed one, if some index is never assigned. |
| Which version communicates "I already know how big this will be"? | The pre-sized version. The growing version communicates "I don't yet know how many values I'll end up with." |

> ### Law of Pre-Sized Lists
> When a list's final length is known before the loop starts, create it at that length and fill it by index; save `append` for when the number of results can't be known in advance.

---

## Part 4.7 - Changing an Existing List

Changing the loop variable is not the same as changing the list.

| Question | Answer |
| :--- | :--- |
| What is `number` during each pass?<br><br>`numbers = [1, 2, 3]`<br>`for number in numbers:`<br>&emsp;`number = number * 2` | `1`, then `2`, then `3`. Each name is changed locally during its pass. |
| What is `numbers` after that loop? | `[1, 2, 3]`. Reassigning `number` does not change the list. |
| Why does the list remain unchanged? | `number` is a name for the current value; it is not a direct instruction to replace the list element. |
| What is `numbers` after this loop?<br><br>`numbers = [1, 2, 3]`<br>`for i in range(len(numbers)):`<br>&emsp;`numbers[i] = numbers[i] * 2` | `[2, 4, 6]`. The list is changed through its indices. |
| What does `i` represent in the second loop? | The index of the element being changed. |
| Which loop changes the original list?<br><br>`for number in numbers:`<br>&emsp;`number = number * 2`<br><br>or<br><br>`for i in range(len(numbers)):`<br>&emsp;`numbers[i] = numbers[i] * 2` | The second loop. It assigns through `numbers[i]`. |

### Fundamental Questions About Mutation Through a Loop

| Question | Answer |
| :--- | :--- |
| Does changing a loop variable always change the container? | No. |

> ### Law of Mutation Through a Loop
> Reassigning the loop variable changes only that temporary name; changing the list itself requires assigning through an index, as in `items[i] = ...`.

| How can a loop replace values in a list? | Use an index and assign to `items[index]`. |
| Why is `range(len(items))` useful here? | It provides every valid index in the list. |
| Is this the only way to change list elements in a loop? | No, but it is the explicit form students should understand before learning shorter tools such as `enumerate`. |

---

## Part 4.8 - `range` as a Counting Structure

`range` can provide values to a loop even when there is no existing list.

| Question | Answer |
| :--- | :--- |
| What values are visited?<br><br>`for x in range(4):`<br>&emsp;`print(x)` | `0`, `1`, `2`, and `3`. |
| How many times does the loop run? | Four times. |
| What values are visited?<br><br>`for x in range(2, 6):`<br>&emsp;`print(x)` | `2`, `3`, `4`, and `5`. The stop value `6` is not included. |
| What values are visited?<br><br>`for x in range(0, 10, 2):`<br>&emsp;`print(x)` | `0`, `2`, `4`, `6`, and `8`. |
| What does `range` provide to the loop? | A generated sequence of integer values. |
| What is the difference between `for item in items` and `for number in range(5)`? | The first visits values already stored in a collection. The second generates counting values for the loop. |

### Fundamental Questions About Counting

| Question | Answer |
| :--- | :--- |
| What does `range(stop)` begin with? | `0`. |
| Is the stop value included? | No. `range` stops before the stop value. |
| How many values does `range(5)` provide? | Five values: `0, 1, 2, 3, 4`. |
| How can `range` repeat an action without using the counter? | Use `_`, as in `for _ in range(3)`. |

---

## Part 4.9 - Nested Loops

A loop can contain another loop. The inner loop completes all of its passes before the outer loop advances.

| Question | Answer |
| :--- | :--- |
| How many times does the outer loop run?<br><br>`for y in range(3):`<br>&emsp;`for x in range(4):`<br>&emsp;&emsp;`print(x, y)` | Three times. `y` takes the values `0`, `1`, and `2`. |
| How many times does the inner loop run for each value of `y`? | Four times. `x` takes the values `0`, `1`, `2`, and `3`. |
| How many coordinate pairs are printed in total? | Twelve. Three outer passes multiplied by four inner passes. |
| Which value changes faster, `x` or `y`? | `x`. It completes its entire inner loop before `y` changes. |
| What are the first two coordinate pairs printed? | `(0, 0)` and `(1, 0)`. |
| What is the first pair printed when `y` becomes `1`? | `(0, 1)`. |

### Fundamental Questions About Nested Repetition

| Question | Answer |
| :--- | :--- |
| What does the outer loop control? | How many groups of inner work occur. |
| What does the inner loop control? | How many actions occur inside each group. |
| How many total inner actions occur when the outer loop runs `H` times and the inner loop runs `W` times? | `H * W` actions. |
| Why are nested loops useful for grids? | They visit every combination of a row value and a column value. |

---

## Part 4.10 - Building a Two-Dimensional List

The two-dimensional comprehension from Chapter 3 can now be expanded into an ordinary process.

| Question | Answer |
| :--- | :--- |
| What is the initial value of `grid`?<br><br>`grid = []` | An empty outer list. |
| What is the initial value of `row` during each outer pass? | An empty inner list. |
| What happens during the inner loop?<br><br>`row.append('.')` | One `'.'` is added for each column. |
| What happens after the inner loop finishes? | The completed row is appended to `grid`. |
| What is the final value of `grid`?<br><br>`grid = []`<br>`for y in range(2):`<br>&emsp;`row = []`<br>&emsp;`for x in range(3):`<br>&emsp;&emsp;`row.append('.')`<br>&emsp;`grid.append(row)` | `[['.', '.', '.'], ['.', '.', '.']]`. |
| How many independent inner lists are created? | Two. The outer loop creates a new `row` for each pass. |
| Which comprehension expresses the same process? | `[['.' for x in range(3)] for y in range(2)]`. |

### Fundamental Questions About Expanded Comprehensions

| Question | Answer |
| :--- | :--- |
| What does the outer loop create? | One independent row during each pass. |
| What does the inner loop create? | The values inside the current row. |
| Why is this form safer than `[[0] * 3] * 2`? | Each outer pass creates a new inner list instead of repeating one shared list reference. |
| What has the loop version made visible? | The order of construction, the row boundary, and the moment each row enters the grid. |

---

## Part 4.11 - Loop or Comprehension?

| Question | Answer |
| :--- | :--- |
| Which form is easier to read when the only goal is to create a transformed list?<br><br>`result = []`<br>`for x in range(5):`<br>&emsp;`result.append(x * 2)`<br><br>or<br><br>`result = [x * 2 for x in range(5)]` | The comprehension is shorter and communicates the collection-building pattern directly. |
| Which form is better when several actions happen during each pass? | The `for` loop. Its body can contain multiple statements. |
| Which form is better when you need to print, update a score, mutate an existing list, or perform a sequence of steps? | The `for` loop. |
| Does a comprehension replace every loop? | No. It expresses one common kind of loop: building a new collection from another sequence. |
| What is the expanded form of `[x + 1 for x in range(4)]`? | `result = []`, followed by a loop that appends `x + 1` for each `x`. |

---

## Part 4.12 - Predict the Result

Do not run these immediately. Predict the final value and the data type.

| Question | Answer |
| :--- | :--- |
| What is `result`?<br><br>`result = []`<br>`for x in range(4):`<br>&emsp;`result.append(x)` | `[0, 1, 2, 3]`, a list. |
| What is `total`?<br><br>`total = 0`<br>`for value in [3, 4, 5]:`<br>&emsp;`total = total + value` | `12`, an integer. |
| What is `items`?<br><br>`items = ['a', 'b', 'c']`<br>`for i in range(len(items)):`<br>&emsp;`items[i] = items[i].upper()` | `['A', 'B', 'C']`, a list. |
| What is `result`?<br><br>`result = []`<br>`for x in range(5):`<br>&emsp;`if x % 2 == 0:`<br>&emsp;&emsp;`result.append(x)` | `[0, 2, 4]`, a list. |
| What is `pairs`?<br><br>`pairs = []`<br>`for y in range(2):`<br>&emsp;`for x in range(3):`<br>&emsp;&emsp;`pairs.append((x, y))` | `[(0, 0), (1, 0), (2, 0), (0, 1), (1, 1), (2, 1)]`, a list of tuples. |
| What is `grid`?<br><br>`grid = []`<br>`for y in range(2):`<br>&emsp;`row = []`<br>&emsp;`for x in range(3):`<br>&emsp;&emsp;`row.append(x + y)`<br>&emsp;`grid.append(row)` | `[[0, 1, 2], [1, 2, 3]]`, a list of lists. |

---

## Chapter Summary

| Question | Answer |
| :--- | :--- |
| What does a `for` loop do? | It visits each value in a sequence and runs a body of code for that value. |
| What does the loop variable represent? | The current value being visited. |
| What is an accumulator? | A variable that carries a partial result from one pass to the next. |
| How does a loop build a new list? | It starts with an empty list and appends a new value during each pass. |
| How does a loop change an existing list? | It assigns through the list's indices. |
| What does `range` provide? | Generated integer values that can control repetition. |
| What does a nested loop do? | It repeats one process inside another process. |
| How is a comprehension related to a loop? | A comprehension is a compact way to express a common collection-building loop. |
| What is the central difference between a comprehension and a loop? | A comprehension describes the result. A loop reveals the process. |

---

**==> A comprehension describes the result. A loop reveals the process. <==**
