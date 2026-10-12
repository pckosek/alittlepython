# Chapter 5: Conditions, Truthiness, and Choice

By now, you already know how `for` loops and ordinary `if` statements work.

This chapter is not a syntax tour.

The important question is:

> How does Python decide which path to take?

A condition is an expression whose value can be interpreted as true or false. Once we understand that, several compact and powerful Python patterns become available:

- truthiness and falsiness;
- Boolean combinations;
- choosing a value with `and` and `or`;
- one-line conditional expressions;
- conditions inside comprehensions and loops; and
- predicates that describe meaningful states.

---

## Part 5.1 - Conditions Are Expressions

Comparisons do not merely control an `if` statement. They produce values.

| Question | Answer |
| :--- | :--- |
| What is the value of `3 > 2`? | `True`. |
| What is the value of `3 < 2`? | `False`. |
| What is the value of `5 == 5`? | `True`. |
| What is the value of `5 != 5`? | `False`. |
| What is the value of `5 != 6`? | `True`. |
| What is the data type of `3 > 2`? | `bool`, Python's Boolean data type. |
| What is the data type of `True`? | `bool`. |
| What is the value of `health > 0` when `health = 10`? | `True`. |
| What is the value of `health > 0` when `health = 0`? | `False`. |
| What is the value of `name == 'Ada'` when `name = 'Ada'`? | `True`. |
| What is the value of `name == 'ada'` when `name = 'Ada'`? | `False`. String comparison is case-sensitive. |

### Fundamental Questions About Boolean Expressions

| Question | Answer |
| :--- | :--- |
| What does a comparison produce? | Either `True` or `False`. |
| Can the result of a comparison be stored? | Yes. For example, `alive = health > 0`. |
| Can a Boolean expression be passed to a function? | Yes. It is an ordinary Python value. |
| What is the difference between `=` and `==`? | `=` assigns a value. `==` compares two values. |
| What is the value of `5 == '5'`? | `False`. The values have different types. |

---

## Part 5.2 - Truthiness and Falsiness

Python allows many values to participate directly in a condition.

Some values behave as false:

```python
False
None
0
''
[]
()
{}
set()
```

Most other values behave as true.

| Question | Answer |
| :--- | :--- |
| What is `bool(False)`? | `False`. |
| What is `bool(True)`? | `True`. |
| What is `bool(0)`? | `False`. |
| What is `bool(7)`? | `True`. |
| What is `bool('')`? | `False`. An empty string is falsy. |
| What is `bool('hello')`? | `True`. A non-empty string is truthy. |
| What is `bool([])`? | `False`. An empty list is falsy. |
| What is `bool([0])`? | `True`. The list contains one element, even though that element is `0`. |
| What is `bool({})`? | `False`. An empty dictionary is falsy. |
| What is `bool({'score': 0})`? | `True`. The dictionary contains a key-value pair. |
| What is `bool(None)`? | `False`. `None` represents the absence of a value. |

### Fundamental Questions About Empty Structures

| Question | Answer |
| :--- | :--- |
| What does `if items:` mean when `items` is a list? | “If `items` is non-empty.” |
| What does `if not items:` mean? | “If `items` is empty.” |
| How can you test whether a dictionary has any entries? | `if player:`. |
| How can you test whether a set has no members? | `if not visited:`. |
| Is `if items == []:` equivalent to `if not items:` for an ordinary list? | Usually yes, but `if not items:` is more general and idiomatic. |
| Does truthiness change the object? | No. It only determines how the object behaves when Python needs a true-or-false decision. |

> ### Law of Empty Structures
> `False`, `None`, `0`, and every empty collection are falsy; nearly everything else, including a list holding only `0`, is truthy.

---

## Part 5.3 - Combining Conditions with `and`

The operator `and` combines conditions that must all be satisfied.

| Question | Answer |
| :--- | :--- |
| What is `True and True`? | `True`. |
| What is `True and False`? | `False`. |
| What is `False and True`? | `False`. |
| What is `False and False`? | `False`. |
| What must be true for an `and` condition to be true? | Every part must be true. |
| What does this condition require?<br><br>`if health > 0 and 'key' in inventory:` | The player must be alive and must have a key. |
| Is the condition true when `health = 10` and `inventory = ['sword', 'key']`? | `True`. Both parts are true. |
| Is it true when `health = 0` and `inventory = ['key']`? | `False`. The health condition is false. |
| Is it true when `health = 10` and `inventory = []`? | `False`. The membership condition is false. |

### Fundamental Questions About `and`

| Question | Answer |
| :--- | :--- |
| What does `and` mean conceptually? | “This condition and that condition must both hold.” |
| What happens when one part of an `and` expression is false? | The whole expression is false. |
| What is `0 < health <= 100` asking? | Whether `health` is greater than `0` and less than or equal to `100`. |
| Why might named Boolean variables improve a complicated condition? | They give meaningful names to the individual ideas being combined. |

---

## Part 5.4 - `or` and `not`

The operator `or` requires at least one condition to be true. The operator `not` reverses truth value.

| Question | Answer |
| :--- | :--- |
| What is `False or False`? | `False`. |
| What is `True or False`? | `True`. |
| What is `False or True`? | `True`. |
| What is `True or True`? | `True`. |
| What must be true for an `or` condition to be true? | At least one part must be true. |
| What is `not True`? | `False`. |
| What is `not False`? | `True`. |
| What does `if 'key' in inventory or 'map' in inventory:` allow? | The player may proceed if they have a key or a map. |
| What does `if not door_open:` mean? | “If the door is not open,” or “if the door is closed.” |

### Fundamental Questions About `or` and `not`

| Question | Answer |
| :--- | :--- |
| What does `or` mean conceptually? | “At least one of these possibilities is true.” |
| When is an `or` expression false? | When every part is false. |
| What does `not` do? | It reverses a Boolean value. |
| Why can `not` sometimes make code harder to read? | A double negative can force the reader to mentally reverse the condition more than once. |

---

## Part 5.5 - `and` and `or` Can Select Values

In Python, `and` and `or` do more than produce `True` or `False`. They can return one of their operands.

| Question | Answer |
| :--- | :--- |
| What is the value of `0 or 5`? | `5`. The first value is falsy, so `or` returns the second value. |
| What is the value of `12 or 5`? | `12`. The first value is truthy, so `or` returns it. |
| What is the value of `'' or 'unknown'`? | `'unknown'`. The empty string is falsy. |
| What is the value of `'Ada' or 'unknown'`? | `'Ada'`. The first string is truthy. |
| What is the value of `[] and 'visible'`? | `[]`. The first value is falsy, so `and` stops and returns it. |
| What is the value of `[1, 2] and 'visible'`? | `'visible'`. The first value is truthy, so `and` returns the second operand. |
| What is `name` after `name = supplied_name or 'Anonymous'` when `supplied_name = ''`? | `'Anonymous'`. |
| What is `name` when `supplied_name = 'Ada'`? | `'Ada'`. |

### Fundamental Questions About Value Selection

| Question | Answer |
| :--- | :--- |
| What does `a or b` return when `a` is truthy? | `a`. |
| What does `a or b` return when `a` is falsy? | `b`. |
| What does `a and b` return when `a` is falsy? | `a`. |
| What does `a and b` return when `a` is truthy? | `b`. |
| Does `and` or `or` necessarily return a Boolean? | No. It may return an original operand such as a string, list, number, or `None`. |
| When is `value = possible_value or default_value` useful? | When a falsy value should be replaced by a fallback. |
| What should you watch for with this pattern? | It treats every falsy value as missing, including `0`, `False`, and `''`. |

> ### Law of Value Selection
> `a or b` returns `a` if `a` is truthy and `b` otherwise; `a and b` returns `a` if `a` is falsy and `b` otherwise — neither necessarily returns a Boolean.

---

## Part 5.6 - One-Line Conditional Expressions

Python can choose between two values in one line.

```python
value_if_true if condition else value_if_false
```

Read this as:

> Use the first value if the condition is true. Otherwise, use the second value.

| Question | Answer |
| :--- | :--- |
| What is `state`?<br><br>`health = 10`<br>`state = 'alive' if health > 0 else 'defeated'` | `'alive'`. |
| What is `state` when `health = 0`? | `'defeated'`. |
| What is `label`?<br><br>`score = 75`<br>`label = 'passing' if score >= 60 else 'failing'` | `'passing'`. |
| What is `label` when `score = 42`? | `'failing'`. |
| What is `color`?<br><br>`is_player = True`<br>`color = 'blue' if is_player else 'red'` | `'blue'`. |
| What is `color` when `is_player = False`? | `'red'`. |
| What is the longer equivalent of `state = 'alive' if health > 0 else 'defeated'`? | An ordinary `if`/`else` that assigns `'alive'` or `'defeated'` to `state`. |
| When should you avoid a one-line conditional? | When either branch is long, nested, or difficult to understand at a glance. |

### Fundamental Questions About Conditional Expressions

| Question | Answer |
| :--- | :--- |
| How many values does a one-line conditional choose between? | Two. |
| Must a one-line conditional include `else`? | Yes. It must define the value for both possible outcomes. |
| Is this a statement or an expression?<br><br>`'alive' if health > 0 else 'defeated'` | An expression. It produces a value. |
| What is the relationship between an `if` statement and a conditional expression? | An `if` statement chooses actions or executes blocks. A conditional expression chooses one value. |

---

## Part 5.7 - Conditions Inside Comprehensions

Chapter 3 introduced filtering in comprehensions. Now we can see the logic more clearly.

| Question | Answer |
| :--- | :--- |
| What values are included?<br><br>`[x for x in range(8) if x % 2 == 0]` | `0`, `2`, `4`, and `6`. |
| What does the condition `x % 2 == 0` decide? | Whether the current `x` is included in the resulting list. |
| What is the difference between these expressions?<br><br>`[x * 2 for x in range(5)]`<br>`[x * 2 for x in range(5) if x % 2 == 0]` | The first transforms every value. The second transforms only even values. |
| What is the value of `result`?<br><br>`numbers = [-2, 3, -1, 5]`<br>`result = [number * 10 for number in numbers if number > 0]` | `[30, 50]`. Only positive numbers enter the expression. |
| What is the value of `labels`?<br><br>`numbers = [1, 2, 3, 4]`<br>`labels = ['even' if number % 2 == 0 else 'odd' for number in numbers]` | `['odd', 'even', 'odd', 'even']`. The conditional expression transforms every value. |
| What is the difference between a comprehension filter and a conditional expression inside a comprehension? | A filter decides whether an item appears. A conditional expression decides what value appears. |

### Fundamental Questions About Two Kinds of Conditional Comprehension

| Question | Answer |
| :--- | :--- |
| What does this form do?<br><br>`[expression for item in sequence if condition]` | It filters. Some items may be excluded. |
| What does this form do?<br><br>`[value_a if condition else value_b for item in sequence]` | It transforms every item, choosing between two output values. |
| Can a comprehension use both forms? | Yes, although the result should remain readable. |

---

## Part 5.8 - Designing Predicates

A predicate is a Boolean expression that describes whether something belongs to a category or satisfies a rule.

| Question | Answer |
| :--- | :--- |
| Is `number > 0` a predicate? | Yes. It describes whether a number is positive. |
| Is `'key' in inventory` a predicate? | Yes. It describes whether the inventory contains a key. |
| Is `len(items) == 0` a predicate? | Yes. It describes whether a collection is empty. |
| What does `is_alive = health > 0` store? | A Boolean value describing the player's state. |
| What does `has_treasure = 'treasure' in inventory` store? | A Boolean value describing the inventory's contents. |
| Why might a named predicate be useful?<br><br>`can_open = has_key and not door_locked` | It gives a meaningful name to a combination of conditions. |
| What does `if can_open:` communicate? | The code is asking whether the door may be opened, without repeating the implementation details. |

### Fundamental Questions About Named Conditions

| Question | Answer |
| :--- | :--- |
| When should a condition receive a name? | When it expresses a meaningful concept or will be used more than once. |
| Is `can_open` the same kind of thing as `health`? | No. `health` stores a quantity. `can_open` stores a Boolean state derived from other information. |
| Can a named predicate become outdated? | Yes. If the underlying data changes, the stored Boolean does not automatically recalculate. |
| What is the difference between these two forms?<br><br>`can_open = has_key and not locked`<br>`if has_key and not locked:` | The first stores the current result. The second tests the current condition at that moment. |

---

## Part 5.9 - Ordering and Overlapping Conditions

In an `if`/`elif` chain, order matters.

| Question | Answer |
| :--- | :--- |
| What is `state`?<br><br>`score = 95`<br>`if score >= 90:`<br>&emsp;`state = 'gold'`<br>`elif score >= 50:`<br>&emsp;`state = 'silver'`<br>`else:`<br>&emsp;`state = 'bronze'` | `'gold'`. The first condition is true. |
| What is `state` when `score = 75`? | `'silver'`. The first condition is false and the second is true. |
| What is `state` when `score = 20`? | `'bronze'`. Both earlier conditions are false. |
| What happens if `score >= 50` is placed before `score >= 90`? | A score of `95` would be classified as silver because the broader condition catches it first. |
| Which condition should usually come first: a specific condition or a broad condition? | The specific condition. |
| Why are mutually exclusive categories useful? | They make it easier to guarantee that each value belongs to exactly one branch. |

### Fundamental Questions About First Truth

| Question | Answer |
| :--- | :--- |
| In what order does Python test an `if`/`elif` chain? | From top to bottom. |
| When does Python stop testing? | When it reaches the first true condition. |
| Can a later `elif` run after an earlier branch has run? | No. |
| What should you ask when debugging a classification chain? | Which conditions overlap, and which one will be tested first? |

> ### Law of First Truth
> An `if`/`elif` chain tests its conditions from top to bottom and commits to the first one that is true — put the most specific condition first.

---

## Part 5.10 - Conditions as State Transitions

Conditions become especially meaningful when they change the state of a system.

| Question | Answer |
| :--- | :--- |
| What is `message`?<br><br>`health = 10`<br>`message = 'alive' if health > 0 else 'defeated'` | `'alive'`. |
| What is `door_open`?<br><br>`door_open = False`<br>`has_key = True`<br>`if has_key:`<br>&emsp;`door_open = True` | `True`. |
| What is `score`?<br><br>`score = 10`<br>`collected_coin = True`<br>`score += 1 if collected_coin else 0` | `11`. |
| What is `visited`?<br><br>`visited = set()`<br>`point = (2, 3)`<br>`if point not in visited:`<br>&emsp;`visited.add(point)` | `{(2, 3)}`. |
| What is `result`?<br><br>`result = []`<br>`for number in [-2, 4, -1, 8]:`<br>&emsp;`result.append('positive' if number > 0 else 'negative')` | `['negative', 'positive', 'negative', 'positive']`. |
| What is the larger pattern in these examples? | A condition inspects the current state and determines the next state or action. |

---

## Part 5.11 - Predict the Result

Do not run these immediately. Predict the final value and the data type.

| Question | Answer |
| :--- | :--- |
| What is `name`?<br><br>`supplied_name = ''`<br>`name = supplied_name or 'Anonymous'` | `'Anonymous'`, a string. |
| What is `name` when `supplied_name = 'Ada'`? | `'Ada'`, a string. |
| What is `value`?<br><br>`items = []`<br>`value = items or ['default']` | `['default']`, a list. |
| What is `value` when `items = ['sword']`? | `['sword']`, a list. |
| What is `label`?<br><br>`number = 7`<br>`label = 'even' if number % 2 == 0 else 'odd'` | `'odd'`, a string. |
| What is `result`?<br><br>`numbers = [-3, 0, 4, 8]`<br>`result = [number for number in numbers if number]` | `[-3, 4, 8]`. `0` is falsy and is filtered out. |
| What is `result`?<br><br>`numbers = [-3, 0, 4, 8]`<br>`result = [number if number > 0 else 0 for number in numbers]` | `[0, 0, 4, 8]`. Every source value produces an output value. |
| What is `can_enter`?<br><br>`health = 10`<br>`inventory = ['map']`<br>`can_enter = health > 0 and ('key' in inventory or 'map' in inventory)` | `True`, a Boolean. |
| What is `state`?<br><br>`score = 85`<br>`if score >= 90:`<br>&emsp;`state = 'gold'`<br>`elif score >= 80:`<br>&emsp;`state = 'silver'`<br>`else:`<br>&emsp;`state = 'bronze'` | `'silver'`, a string. |

---

## Chapter Summary

| Question | Answer |
| :--- | :--- |
| What is a Boolean expression? | An expression that can be interpreted as true or false. |
| What does truthiness mean? | Python's ability to treat a value as true or false in a condition. |
| Which common values are falsy? | `False`, `None`, `0`, empty strings, and empty collections. |
| What does `and` require? | Every condition must be true. It can also select and return an operand. |
| What does `or` require? | At least one condition must be true. It can also select and return an operand. |
| What does `not` do? | It reverses the truth value of an expression. |
| What is a conditional expression? | A one-line expression that chooses one of two values. |
| What is the difference between filtering and conditional transformation? | Filtering may remove values. Conditional transformation produces one output for every input. |
| What is a predicate? | A Boolean expression that describes whether a rule or category applies. |
| What is the central idea? | Conditions do not merely control code. They describe the state of a system and determine how that state changes. |

---

**==> A condition describes what is true now. A decision describes what should happen next. <==**
