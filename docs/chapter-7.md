# Chapter 7: Moving Several Values Together

In Chapter 6, a function gave a name to a process.

```python
def double(number):
    return number * 2
```

Now we ask:

> What happens when a process needs several related values at the same time?

Python gives us several ways to keep values together without losing their relationships:

- unpacking;
- `enumerate`;
- `zip`;
- dictionary keys and values;
- dictionary `.items()`; and
- unpacking arguments into functions.

The point is not to learn several disconnected tricks. The point is to preserve relationships while data moves through a program.

---

## Part 7.1 - Unpacking a Sequence

| Question | Answer |
| :--- | :--- |
| True or false: a tuple can provide several values to several names in one assignment.<br><br>`x, y = (3, 4)` | **True.** `x` becomes `3` and `y` becomes `4`. |
| What is `x` after `x, y = (3, 4)`? | `3`. |
| What is `y`? | `4`. |
| Is `x, y = (3, 4)` the same as `x = (3, 4)`? | **No.** The first unpacks two values into two names. The second stores the entire tuple in `x`. |
| Can a list be unpacked?<br><br>`first, second = ['sword', 'shield']` | Yes. `first` becomes `'sword'`; `second` becomes `'shield'`. |
| What happens when there are too many names?<br><br>`a, b = [1, 2, 3]` | Python raises a `ValueError`. There are more values than names. |
| What happens when there are too few names?<br><br>`a, b, c = [1, 2]` | Python raises a `ValueError`. There are fewer values than names. |
| What does `x, y = point` communicate when `point = (3, 4)`? | That the two parts of the point have meaningful names. |

### Fundamental Questions About Matching Parts

| Question | Answer |
| :--- | :--- |
| What must match during ordinary unpacking? | The number of values and the number of receiving names. |
| What does the order of the names determine? | Which value each name receives. |
| Does unpacking change the original sequence? | No. It creates names that refer to the existing values. |
| Why is unpacking useful? | It reveals the structure of a compound value without repeatedly using numeric indices. |

> ### Law of Matching Parts
> Unpacking assignment requires the number of names to match the number of values; it names the parts of a sequence without changing the sequence itself.

---

## Part 7.2 - The Rest of a Sequence

The star operator can collect several values into one list.

| Question | Answer |
| :--- | :--- |
| What is `first` and what is `rest`?<br><br>`first, *rest = [10, 20, 30, 40]` | `first` is `10`; `rest` is `[20, 30, 40]`. |
| What is `last` and what is `beginning`?<br><br>`*beginning, last = [10, 20, 30, 40]` | `beginning` is `[10, 20, 30]`; `last` is `40`. |
| What is `middle`?<br><br>`first, *middle, last = [10, 20, 30, 40]` | `[20, 30]`. |
| What is `rest`?<br><br>`first, *rest = [10]` | `[]`. The starred name receives the remaining values, even when there are none. |
| Can there be two starred names in one unpacking assignment? | No. Python would not know how to divide the remaining values between them. |
| What type of object does a starred receiving name collect? | A list. |

### Fundamental Questions About the Starred Name

| Question | Answer |
| :--- | :--- |
| What does a starred receiving name mean? | “Collect all remaining values here.” |
| Does the starred name receive one value or a list? | A list containing the remaining values. |
| Why is `first, *rest` useful for sequences? | It separates the first element from everything that follows without manually slicing. |

---

## Part 7.3 - Unpacking Inside a Loop

If every element of a sequence has the same structure, the loop can unpack each element as it arrives.

```python
points = [(2, 4), (5, 1), (3, 3)]

for x, y in points:
    print(x, y)
```

| Question | Answer |
| :--- | :--- |
| True or false: the loop variable in `for x, y in points` is one name. | **False.** Each point is unpacked into two names. |
| What are `x` and `y` during the first pass? | `x = 2`, `y = 4`. |
| What are `x` and `y` during the second pass? | `x = 5`, `y = 1`. |
| What are `x` and `y` during the third pass? | `x = 3`, `y = 3`. |
| What happens if one point has three values?<br><br>`points = [(2, 4), (5, 1, 8)]` | The loop raises a `ValueError` when it reaches the second point. |
| What is the advantage of `for x, y in points` over `for point in points` followed by `point[0]` and `point[1]`? | The structure and meaning of each component are visible immediately. |

### Fundamental Questions About Structured Iteration

| Question | Answer |
| :--- | :--- |
| What does the loop expect from each element? | An unpackable structure with the correct number of parts. |
| Does unpacking remove the tuple from the original list? | No. It only gives names to its parts during the current pass. |
| What does `for name, score in records` communicate? | Each record contains two related values: a name and a score. |

---

## Part 7.4 - `enumerate`

Sometimes a loop needs both an element and its position.

| Question | Answer |
| :--- | :--- |
| True or false: `for item in items` automatically gives you the item's index. | **False.** It gives you the item, not its position. |
| What does `enumerate(items)` provide? | Pairs containing an index and an item. |
| What values are produced?<br><br>`items = ['sword', 'shield', 'potion']`<br>`for index, item in enumerate(items):`<br>&emsp;`print(index, item)` | `(0, 'sword')`, `(1, 'shield')`, and `(2, 'potion')`. |
| What are `index` and `item` during the second pass? | `index = 1`; `item = 'shield'`. |
| What is the first index produced by `enumerate(items)`? | `0`. |
| Can enumeration begin at another number?<br><br>`enumerate(items, start=1)` | Yes. The pairs begin with index `1`. |
| What values are produced by `enumerate(['a', 'b'], start=1)`? | `(1, 'a')` and `(2, 'b')`. |

### Fundamental Questions About Position and Value

| Question | Answer |
| :--- | :--- |
| When should you use `enumerate`? | When the loop needs both the current value and its position. |
| Is `enumerate(items)` a list? | No. It produces an iterable of index-value pairs. |
| What does `for i in range(len(items))` provide? | Indices only. |
| What does `for i, item in enumerate(items)` provide? | Both the index and the item. |
| Why is `enumerate` safer than manually managing a counter? | Python keeps the index synchronized with the value being visited. |

> ### Law of Position and Value
> `enumerate(items)` pairs each value with its position; reach for it whenever a loop needs both.

---

## Part 7.5 - `zip`

Sometimes two sequences contain related values stored in separate places.

```python
names = ['Ada', 'Grace', 'Alan']
scores = [95, 88, 91]
```

| Question | Answer |
| :--- | :--- |
| True or false: `zip(names, scores)` pairs the first name with the first score. | **True.** |
| What pair is produced first? | `('Ada', 95)`. |
| What pair is produced second? | `('Grace', 88)`. |
| What pair is produced third? | `('Alan', 91)`. |
| What does this loop print?<br><br>`for name, score in zip(names, scores):`<br>&emsp;`print(name, score)` | `Ada 95`, then `Grace 88`, then `Alan 91`. |
| What happens if one sequence is shorter?<br><br>`zip(['a', 'b', 'c'], [1, 2])` | It stops when the shortest input is exhausted. The unmatched `'c'` is ignored. |
| What does `list(zip(['a', 'b'], [1, 2]))` produce? | `[('a', 1), ('b', 2)]`. |
| Does `zip` change either original list? | No. It creates paired values for iteration. |

### Fundamental Questions About Pairing

| Question | Answer |
| :--- | :--- |
| What does `zip` do? | It joins values from corresponding positions in multiple sequences. |
| What determines which values are paired? | Their positions in the input sequences. |
| What happens when the input lengths differ? | Ordinary `zip` stops at the shortest sequence. |
| When is `zip` useful? | When related data is stored in parallel sequences or when several values should be processed together. |

> ### Law of Pairing
> `zip` pairs values from matching positions across sequences and stops at the shortest one — it never pads or repeats to make lengths agree.

---

## Part 7.6 - Building Dictionaries with `zip`

Paired values can become dictionary entries.

| Question | Answer |
| :--- | :--- |
| What is `list(zip(names, scores))` when `names = ['Ada', 'Grace']` and `scores = [95, 88]`? | `[('Ada', 95), ('Grace', 88)]`. |
| What is `dict(zip(names, scores))`? | `{'Ada': 95, 'Grace': 88}`. |
| What becomes the dictionary keys? | The values from the first sequence, `names`. |
| What becomes the dictionary values? | The values from the second sequence, `scores`. |
| What happens when duplicate keys are supplied?<br><br>`dict(zip(['a', 'a'], [1, 2]))` | The later value wins: `{'a': 2}`. |
| What happens when the sequences have different lengths? | Only pairs produced by `zip` become dictionary entries. |
| Can a list be used as a key in `dict(zip(keys, values))`? | No. The keys must be hashable. |

### Fundamental Questions About Relationship Construction

| Question | Answer |
| :--- | :--- |
| What does `zip` provide? | Pairs or groups of related values. |
| What does `dict` do with pairs? | It interprets each pair as a key and a value. |
| What larger idea appears here? | A sequence of related values can become a structure that gives those relationships names. |

---

## Part 7.7 - Iterating Through Dictionaries

Dictionaries can be visited in several ways, depending on what the process needs.

| Question | Answer |
| :--- | :--- |
| What does `for key in player` visit?<br><br>`player = {'health': 100, 'coins': 25}` | The keys: `'health'` and `'coins'`. |
| What does `for value in player.values()` visit? | The values: `100` and `25`. |
| What does `for key, value in player.items()` visit? | Key-value pairs: `('health', 100)` and `('coins', 25)`. |
| What are `key` and `value` during the first `.items()` pass? | `key = 'health'`; `value = 100`. |
| What is printed?<br><br>`for key, value in player.items():`<br>&emsp;`print(key, value)` | `health 100`, then `coins 25`. |
| Can `.items()` be unpacked in a loop? | Yes. Each dictionary item is a two-part key-value pair. |
| Does `for value in player` visit the values? | No. Iterating over a dictionary directly visits its keys. |

### Fundamental Questions About Dictionary Views

| Question | Answer |
| :--- | :--- |
| When should you iterate over a dictionary directly? | When you need its keys. |
| When should you use `.values()`? | When you need its values but not the keys. |
| When should you use `.items()`? | When you need keys and values together. |
| Why is `for key, value in player.items()` expressive? | It states directly that the process needs both parts of each dictionary entry. |

---

## Part 7.8 - Updating a Dictionary While Iterating

The key-value relationship can be used to create a new dictionary or update existing values carefully.

| Question | Answer |
| :--- | :--- |
| What is `result`?<br><br>`scores = {'Ada': 90, 'Grace': 80}`<br>`result = {name: score + 5 for name, score in scores.items()}` | `{'Ada': 95, 'Grace': 85}`. |
| What does `name` represent during each pass? | The current dictionary key. |
| What does `score` represent? | The value associated with the current key. |
| What is `passing`?<br><br>`scores = {'Ada': 90, 'Grace': 45, 'Alan': 75}`<br>`passing = {name: score for name, score in scores.items() if score >= 60}` | `{'Ada': 90, 'Alan': 75}`. |
| Does the comprehension change `scores`? | No. It creates a new dictionary. |
| What is the dictionary version of filtering key-value pairs? | `{key: value for key, value in dictionary.items() if condition}`. |

---

## Part 7.9 - Unpacking Arguments into a Function

The star operator can unpack a sequence when calling a function.

```python
def move(x, y):
    return (x + 1, y)

point = (3, 4)
move(*point)
```

| Question | Answer |
| :--- | :--- |
| True or false: `move(*point)` passes the tuple as one argument. | **False.** The star expands the tuple into separate positional arguments. |
| What call is equivalent to `move(*point)` when `point = (3, 4)`? | `move(3, 4)`. |
| What is the result of `move(*point)`? | `(4, 4)`. |
| What happens with `move(point)`? | Python raises a `TypeError` because the function expects two arguments, not one tuple. |
| What happens with `move(*(3, 4))`? | It is equivalent to `move(3, 4)`. |
| What does the star mean in this context? | “Unpack this sequence into positional arguments.” |

### Fundamental Questions About Positional Expansion

| Question | Answer |
| :--- | :--- |
| What must be true for `function(*values)` to work? | The number of unpacked values must match the function's positional parameters, unless the function accepts a variable number of arguments. |
| Does `*` create a new conceptual value here? | No. It changes how the existing sequence is supplied to the call. |
| Why is argument unpacking useful with coordinates? | A coordinate tuple can move directly into a function that expects separate coordinate arguments. |

---

## Part 7.10 - Unpacking Dictionary Arguments

Two stars unpack dictionary entries into keyword arguments.

```python
def describe(name, score):
    return name + ': ' + str(score)

record = {'name': 'Ada', 'score': 95}
```

| Question | Answer |
| :--- | :--- |
| What is `describe(**record)` equivalent to? | `describe(name='Ada', score=95)`. |
| What is the result of `describe(**record)`? | `'Ada: 95'`. |
| What does `**` require when used in a function call? | A dictionary whose keys match keyword parameter names. |
| What happens if `record` contains an unexpected key?<br><br>`{'name': 'Ada', 'score': 95, 'level': 3}` | The call raises a `TypeError` unless the function accepts that extra keyword. |
| What happens if a required parameter is missing? | The call raises a `TypeError`. |
| What is the difference between `*values` and `**values` in a call? | `*` supplies positional arguments; `**` supplies keyword arguments. |

---

## Part 7.11 - Combining the Tools

The value of these tools appears when they work together.

| Question | Answer |
| :--- | :--- |
| What does this loop provide?<br><br>`for index, (x, y) in enumerate(points):` | An index and the two unpacked coordinates of each point. |
| What is printed?<br><br>`names = ['Ada', 'Grace']`<br>`scores = [95, 88]`<br>`for index, (name, score) in enumerate(zip(names, scores)):`<br>&emsp;`print(index, name, score)` | `0 Ada 95`, then `1 Grace 88`. |
| What is `labels`?<br><br>`records = [('Ada', 95), ('Grace', 88)]`<br>`labels = [name + ': ' + str(score) for name, score in records]` | `['Ada: 95', 'Grace: 88']`. |
| What is `result`?<br><br>`points = [(2, 4), (5, 1)]`<br>`result = [move(*point) for point in points]`<br><br>where `move(x, y)` returns `(x + 1, y)` | `[(3, 4), (6, 1)]`. |
| What is the larger pattern? | Structured values can be unpacked, processed by functions, and recombined without losing their relationships. |

---

## Part 7.12 - Predict the Result

Do not run these immediately. Predict the final value and the data type.

| Question | Answer |
| :--- | :--- |
| What are `x` and `y`?<br><br>`x, y = (8, 3)` | `x = 8`; `y = 3`. |
| What are `first` and `rest`?<br><br>`first, *rest = ['a', 'b', 'c']` | `first = 'a'`; `rest = ['b', 'c']`. |
| What is `result`?<br><br>`items = ['a', 'b', 'c']`<br>`result = [(i, item) for i, item in enumerate(items)]` | `[(0, 'a'), (1, 'b'), (2, 'c')]`, a list of tuples. |
| What is `result`?<br><br>`result = list(zip([1, 2, 3], ['a', 'b', 'c']))` | `[(1, 'a'), (2, 'b'), (3, 'c')]`, a list of tuples. |
| What is `result`?<br><br>`scores = {'Ada': 90, 'Grace': 80}`<br>`result = [name for name, score in scores.items() if score >= 85]` | `['Ada']`, a list. |
| What is `result`?<br><br>`def move(x, y):`<br>&emsp;`return (x + 1, y)`<br><br>`point = (2, 5)`<br>`result = move(*point)` | `(3, 5)`, a tuple. |
| What is `result`?<br><br>`def label(name, score):`<br>&emsp;`return name + str(score)`<br><br>`record = {'name': 'A', 'score': 7}`<br>`result = label(**record)` | `'A7'`, a string. |
| What is `result`?<br><br>`names = ['Ada', 'Grace', 'Alan']`<br>`scores = [95, 88]`<br>`result = dict(zip(names, scores))` | `{'Ada': 95, 'Grace': 88}`. The unmatched `'Alan'` is ignored. |

---

## Chapter Summary

| Question | Answer |
| :--- | :--- |
| What is unpacking? | Giving separate names to the parts of a sequence. |
| What does `enumerate` provide? | An index and an item together. |
| What does `zip` provide? | Corresponding values from multiple sequences together. |
| What does dictionary iteration provide by default? | Keys. |
| What does `.values()` provide? | Dictionary values. |
| What does `.items()` provide? | Key-value pairs that can be unpacked. |
| What does `*values` do in a function call? | Expands a sequence into positional arguments. |
| What does `**values` do in a function call? | Expands a dictionary into keyword arguments. |
| What is the central idea? | Related values can move together through unpacking, iteration, dictionaries, and function calls. |

---

**==> When values belong together, let the code carry them together. <==**
