# Chapter 3: Comprehensions

The first chapters asked you to recognize Python's containers and change them deliberately.

This chapter asks a different question:

> How can Python build a new collection by repeating a simple rule?

A comprehension is a compact way to describe that repetition. It combines four ideas:

1. Start with a sequence of values.
2. Take one value at a time.
3. Build a new value from it.
4. Collect the results into a new structure.

The basic pattern is:

```python
[expression for item in sequence]
```

Read this as:

> Make a list containing `expression` for every `item` in `sequence`.

---

## Part 3.1 - List Comprehensions: Inference

For each question, write the list comprehension that would generate the requested list. Do not run the code until you have made a prediction.

| Question | Answer |
| :--- | :--- |
| Write the list comprehension that would generate `[0, 1, 2, 3, 4]`. | `[x for x in range(5)]` |
| Write the list comprehension that would generate `[5, 6, 7, 8, 9]` without adding `5` to each value. | `[x for x in range(5, 10)]` |
| Write the list comprehension that would generate `[0.0, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9]`. Do not use a decimal step inside `range`. | `[x / 10 for x in range(10)]` |
| Write the list comprehension that would generate `[0, 2, 4, 6, 8]`. | `[x * 2 for x in range(5)]` |
| Write the list comprehension that would generate four copies of the string `"wall"`. | `["wall" for _ in range(4)]` |
| Write the list comprehension that would generate `[1, 4, 9, 16, 25]`. | `[x ** 2 for x in range(1, 6)]` |

### Fundamental Questions About List Comprehension

| Question | Answer |
| :--- | :--- |
| In `[expression for item in sequence]`, how many times is `expression` evaluated? | Once for every item in `sequence`. |
| What does the sequence after `in` control? | It controls which values are visited and how many times the expression runs. |
| What does `expression` control? | It controls what value is placed into the new list. |
| Why is `_` used in `["wall" for _ in range(4)]`? | `_` communicates that the repeated value is not needed. We only care about repeating the expression four times. |
| What is the difference between `[x for x in range(5)]` and `[x * 2 for x in range(5)]`? | They visit the same source values, but the second expression transforms each value before storing it. |

> ### Law of the Comprehension
> `[expression for item in sequence]` runs `expression` once for every item in `sequence` — the sequence decides how many times, the expression decides what is stored.

---

## Part 3.2 - List Comprehensions: Evaluation

For each question, write the final value of `x` before running the code.

| Question | Answer |
| :--- | :--- |
| What is the value of `x`?<br><br>`x = [x for x in range(4)]` | `[0, 1, 2, 3]` |
| What is the value of `x`?<br><br>`x = [x * 3 for x in range(4)]` | `[0, 3, 6, 9]` |
| What is the value of `x`?<br><br>`x = [x - 1 for x in range(5)]` | `[-1, 0, 1, 2, 3]` |
| What is the value of `x`?<br><br>`x = ["tile" for _ in range(3)]` | `["tile", "tile", "tile"]` |
| What is the value of `x`?<br><br>`words = ["cat", "horse", "owl"]`<br>`x = [len(word) for word in words]` | `[3, 5, 3]` |
| What is the value of `x`?<br><br>`letters = ["a", "b", "c"]`<br>`x = [letter.upper() for letter in letters]` | `["A", "B", "C"]` |

### Fundamental Questions About the Source Sequence

| Question | Answer |
| :--- | :--- |
| How could you expand `[x * 10 for x in range(4)]` before calculating it? | `[0 * 10, 1 * 10, 2 * 10, 3 * 10]` |
| What does that expanded expression become? | `[0, 10, 20, 30]` |
| If the source sequence has six values, how many values can the resulting list contain when there is no `if` clause? | Six values. The expression runs once for every source value. |

---

## Part 3.3 - Transforming an Existing List

The sequence in a comprehension does not have to be a `range`. It can be a list that already exists.

| Question | Answer |
| :--- | :--- |
| Given `numbers = [2, 4, 6, 8]`, write a comprehension that creates a new list containing each number divided by `2`. | `[number / 2 for number in numbers]`, which produces `[1.0, 2.0, 3.0, 4.0]`. |
| Given `names = ["ada", "grace", "alan"]`, write a comprehension that creates a new list containing the names in uppercase letters. | `[name.upper() for name in names]`, which produces `["ADA", "GRACE", "ALAN"]`. |
| Given `words = ["wall", "floor", "door"]`, write a comprehension that creates a new list containing the length of each word. | `[len(word) for word in words]`, which produces `[4, 5, 4]`. |
| Given `points = [(2, 4), (5, 1), (3, 3)]`, write a comprehension that creates a new list containing only the first coordinate from each point. | `[point[0] for point in points]`, which produces `[2, 5, 3]`. |
| Given `points = [(2, 4), (5, 1), (3, 3)]`, write a comprehension that creates a new list containing the sum of the two coordinates in each point. | `[point[0] + point[1] for point in points]`, which produces `[6, 6, 6]`. |

### Fundamental Questions About Transformation

| Question | Answer |
| :--- | :--- |
| When reading `[len(word) for word in ["cat", "horse", "owl"]]`, what values does the source sequence provide? | The source provides the three strings `"cat"`, `"horse"`, and `"owl"`. |
| What does the expression `len(word)` do to each source value? | It converts each string into an integer representing its length. |
| What is the resulting list? | `[3, 5, 3]`. |
| Does this comprehension change the original list? | No. It creates a new list. |

---

## Part 3.4 - Filtering with `if`

A comprehension can decide whether to include an item.

The pattern becomes:

```python
[expression for item in sequence if condition]
```

Read this as:

> For every item in the sequence, include the expression only if the condition is true.

| Question | Answer |
| :--- | :--- |
| What is the value of `x`?<br><br>`x = [x for x in range(8) if x % 2 == 0]` | `[0, 2, 4, 6]` |
| What is the value of `x`?<br><br>`x = [x for x in range(8) if x % 2 == 1]` | `[1, 3, 5, 7]` |
| What is the value of `x`?<br><br>`numbers = [3, 8, 1, 10, 4]`<br>`x = [number for number in numbers if number > 4]` | `[8, 10]` |
| What is the value of `x`?<br><br>`words = ["cat", "horse", "owl", "elephant"]`<br>`x = [word for word in words if len(word) <= 3]` | `["cat", "owl"]` |
| Write a comprehension that creates a list containing only the positive numbers from `numbers = [-3, 4, -1, 8, 0, 2]`. | `[number for number in numbers if number > 0]`, which produces `[4, 8, 2]`. |

### Fundamental Questions About Filtering

| Question | Answer |
| :--- | :--- |
| In `[x * 2 for x in range(5)]`, what does the expression do? | It transforms every source value by multiplying it by `2`. |
| In `[x * 2 for x in range(5) if x % 2 == 0]`, what does the `if` clause do? | It keeps only the even source values. |
| Does the `if` clause transform the value? | No. It decides whether the value is included. The expression decides what gets stored. |
| Can a comprehension with an `if` clause produce fewer values than its source sequence? | Yes. Any source value that fails the condition contributes nothing to the new list. |

> ### Law of Filtering
> An `if` clause in a comprehension decides whether a value survives; the expression decides what it becomes. The two jobs are separate.

---

## Part 3.5 - Building a Two-Dimensional List

So far, every comprehension has built one list. A comprehension can also build a list containing other lists.

| Question | Answer |
| :--- | :--- |
| What is the value of `grid`?<br><br>`grid = [[0 for x in range(3)] for y in range(2)]` | `[[0, 0, 0], [0, 0, 0]]` |
| How many rows does that grid contain? | Two rows. The outer comprehension runs twice. |
| How many values are in each row? | Three values. The inner comprehension runs three times for each row. |
| Write a comprehension that creates a grid with three rows and four `"."` values in each row. | `[["." for x in range(4)] for y in range(3)]` |
| What value is produced by `grid` if `grid = [["." for x in range(4)] for y in range(3)]`? | `[[".", ".", ".", "."], [".", ".", ".", "."], [".", ".", ".", "."]]` |
| Write a comprehension that creates a `5 x 5` checkerboard containing `0` and `1`. | `[[(x + y) % 2 for x in range(5)] for y in range(5)]` |

### Fundamental Questions About Independent Rows

| Question | Answer |
| :--- | :--- |
| What does `grid = [[0] * 4] * 3` appear to create? | Three rows, each containing four zeroes. |
| What is the hidden problem with `grid = [[0] * 4] * 3`? | Each row refers to the same inner list. The rows are not independent. |
| What happens after `grid[0][0] = 1`? | The first value in every row can appear to change, because every row refers to the same list. |
| Which expression creates independent rows?<br><br>`a = [[0] * 4] * 3`<br>`b = [[0 for _ in range(4)] for _ in range(3)]` | `b` creates each row separately. |
| Why does the second comprehension create independent rows? | The outer comprehension evaluates a new inner list expression for every row. |

> ### Law of Independent Rows
> `[[0] * W] * H` builds `H` references to the *same* inner list; only `[[0 for _ in range(W)] for _ in range(H)]` builds `H` genuinely separate rows.

---

## Part 3.6 - A Final Comparison

| Question | Answer |
| :--- | :--- |
| Do these lists contain the same values?<br><br>`a = [0 for _ in range(4)]`<br>`b = [0] * 4` | Yes. Both contain `[0, 0, 0, 0]`. |
| What is the important difference between the two expressions when their elements are simple integers? | They produce the same values. The comprehension makes the repetition rule more visible. |
| What stays the same between `a = [x for x in range(5)]` and `b = [x * 2 for x in range(5)]`? | Both visit the same source values: `0, 1, 2, 3, 4`. |
| What changes between those two comprehensions? | The expression changes. The second one transforms each source value by multiplying it by `2`. |
| What does the `if` clause change in `b = [x for x in range(6) if x % 2 == 0]`? | It changes which source values are included. The result contains only even values. |
| Which version gives each row its own independent list?<br><br>`a = [[0] * 3] * 2`<br>`b = [[0 for _ in range(3)] for _ in range(2)]` | `b` gives each row its own independent list. |

---

## Chapter Summary

| Part of the comprehension | Question to ask |
| :--- | :--- |
| `expression` | What value will be placed in the new list? |
| `item` | What name represents the current value? |
| `sequence` | What values will be examined? |
| `if condition` | Which values will be included? |

| Question | Answer |
| :--- | :--- |
| How can `[x * 2 for x in range(4)]` be expanded? | `[0 * 2, 1 * 2, 2 * 2, 3 * 2]` |
| What does the expanded expression become? | `[0, 2, 4, 6]` |
| What is the central idea of a comprehension? | A comprehension describes how to make a new collection by applying a rule to values from an existing sequence. |

---

**==> You are no longer only storing data. You are describing how data should be made. <==**
