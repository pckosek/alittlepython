# Chapter 8: Randomness and Generation

In Chapter 7, we learned how related values can move together.

Now we ask:

> What if the program should choose among possibilities for us?

Randomness introduces variation. It can choose a location, select an item, vary a number, shuffle an order, or decide whether an event occurs.

But randomness is not the same as chaos.

```python
import random
```

Python gives us tools for making controlled choices. The important questions are:

- What kind of value can be generated?
- What are the boundaries?
- Can the result be repeated?
- Are choices made with or without replacement?
- How can randomness be combined with conditions and data structures?

---

## Part 8.1 - A Random Decimal

The function `random.random()` produces a decimal value between `0.0` and `1.0`.

| Question | Answer |
| :--- | :--- |
| True or false: `random.random()` always returns the same value. | **False.** It normally produces a different pseudo-random value each time it is called. |
| What is the smallest possible value from `random.random()`? | `0.0` is possible. |
| Can `random.random()` return exactly `1.0`? | No. Its values are less than `1.0`. |
| What is the data type returned by `random.random()`? | `float`. |
| Is `random.random() > 0.5` a Boolean expression? | Yes. It asks whether the generated value is greater than `0.5`. |
| What is the approximate chance that `random.random() < 0.25` is true? | Approximately `25%`, assuming a uniform distribution. |
| What is `random.random() < 0.0`? | Always `False`. The generated value cannot be less than `0.0`. |
| What is `random.random() < 1.0`? | Always `True`. The generated value is always less than `1.0`. |

### Fundamental Questions About Random Decimals

| Question | Answer |
| :--- | :--- |
| What interval does `random.random()` use? | `0.0 <= value < 1.0`. |
| Does calling it produce a Boolean? | No. It produces a decimal. A comparison using it may produce a Boolean. |
| How can you turn it into a fifty-percent decision? | Use `random.random() < 0.5`. |
| How can you turn it into a ten-percent decision? | Use `random.random() < 0.1`. |
| What does a probability threshold represent? | The fraction of the interval that counts as success. |

---

## Part 8.2 - Random Integers

`random.randint(a, b)` chooses an integer from `a` through `b`, including both endpoints.

| Question | Answer |
| :--- | :--- |
| True or false: `random.randint(1, 6)` can return `6`. | **True.** The upper endpoint is included. |
| Can `random.randint(1, 6)` return `0`? | No. `0` is outside the inclusive range. |
| What values can `random.randint(3, 5)` produce? | `3`, `4`, or `5`. |
| What is the data type returned by `random.randint(3, 5)`? | `int`. |
| How could you simulate a six-sided die roll? | `random.randint(1, 6)`. |
| What happens when `random.randint(5, 1)` is called? | Python raises a `ValueError` because the lower bound is greater than the upper bound. |
| Is `random.randint(1, 1)` valid? | Yes. It always returns `1`. |

### Fundamental Questions About Inclusive Endpoints

| Question | Answer |
| :--- | :--- |
| Is the lower bound included in `randint(a, b)`? | Yes. |
| Is the upper bound included? | Yes. |
| Why is this different from `range(a, b)`? | `range` excludes its stop value; `randint` includes both endpoints. |
| What is a common mistake with dice or array indices? | Forgetting that `randint` includes its upper bound, which may produce an index one too large. |

> ### Law of Inclusive Endpoints
> `random.randint(a, b)` includes both `a` and `b`; `random.randrange` and `range` follow the same exclusive-stop rule as ordinary slicing.

---

## Part 8.3 - `randrange`

`random.randrange` follows the same stopping rule as `range`: the stop value is excluded.

| Question | Answer |
| :--- | :--- |
| True or false: `random.randrange(6)` can return `6`. | **False.** It can return `0` through `5`. |
| What values can `random.randrange(6)` produce? | `0, 1, 2, 3, 4, 5`. |
| What values can `random.randrange(2, 6)` produce? | `2, 3, 4, 5`. |
| What values can `random.randrange(0, 10, 2)` produce? | `0, 2, 4, 6, 8`. |
| Why is `randrange(len(items))` useful? | It produces valid list indices from `0` through the last index. |
| What is the difference between `randint(0, 5)` and `randrange(6)`? | For these bounds, they can produce the same values, but `randint` emphasizes inclusive endpoints while `randrange` follows `range` semantics. |

### Fundamental Questions About Random Ranges

| Question | Answer |
| :--- | :--- |
| What does `randrange(stop)` exclude? | The stop value. |
| Which function is often safer for a random list index? | `randrange(len(items))`, because its largest possible index is `len(items) - 1`. |
| Can `randrange` use a step? | Yes. It accepts the same start, stop, and step pattern as `range`. |

---

## Part 8.4 - Choosing from a Sequence

`random.choice(sequence)` chooses one existing element.

| Question | Answer |
| :--- | :--- |
| True or false: `random.choice(items)` creates a new item. | **False.** It returns one element already present in `items`. |
| What can `random.choice(['sword', 'shield', 'potion'])` return? | `'sword'`, `'shield'`, or `'potion'`. |
| Can `random.choice` select from a tuple? | Yes. |
| Can `random.choice` select from a string? | Yes. It returns one character. |
| What happens when `random.choice([])` is called? | Python raises an `IndexError` because there is nothing to choose. |
| What is the data type of `random.choice([10, 20, 30])`? | `int`. It returns an element of the input sequence. |
| Can the same value be chosen on two consecutive calls? | Yes. Each call chooses independently from the sequence. |

### Fundamental Questions About Choice

| Question | Answer |
| :--- | :--- |
| What does `choice` require? | A non-empty sequence. |
| Does `choice` remove the selected item? | No. The sequence remains unchanged. |
| What does choosing with replacement mean? | After an item is chosen, it remains available for a later choice. |
| Which operation removes the selected item from a list? | `pop`, not `choice`. |

---

## Part 8.5 - Choosing Without Replacement

`random.sample` chooses several distinct elements without modifying the original sequence.

| Question | Answer |
| :--- | :--- |
| True or false: `random.sample(items, 2)` can return the same list element twice. | **False.** It chooses distinct positions without replacement. |
| What does `random.sample(['a', 'b', 'c'], 2)` return? | A two-element list containing two different elements from the source. |
| Does `sample` change the source list? | No. |
| What happens when the sample size is larger than the sequence?<br><br>`random.sample([1, 2], 3)` | Python raises a `ValueError`. There are not enough elements. |
| What is the data type returned by `sample`? | `list`. |
| When might `sample` be useful in a game? | Choosing several distinct spawn locations, cards, enemies, or rewards. |

### Fundamental Questions About Replacement

| Question | Answer |
| :--- | :--- |
| What does `choice` do with the selected element? | It leaves it available for another choice. |
| What does `sample` do with selected elements? | It treats each source position as available only once during that sample. |
| Does `sample` guarantee that duplicate values cannot appear if the source already contains duplicates? | No. It chooses distinct positions. A source such as `[1, 1, 2]` may produce two equal values from different positions. |

---

## Part 8.6 - Shuffling a Sequence

`random.shuffle(items)` rearranges a mutable sequence in place.

| Question | Answer |
| :--- | :--- |
| True or false: `random.shuffle(items)` returns the shuffled list. | **False.** It changes the list in place and returns `None`. |
| What is guaranteed about the elements after a shuffle? | The same elements remain, but their order may change. |
| Does shuffling a tuple work? | No. Tuples are immutable. |
| What is the correct pattern?<br><br>`items = ['a', 'b', 'c']`<br>`random.shuffle(items)` | The list itself is changed. Do not assign the result of `shuffle` back to `items`. |
| What is the common mistake?<br><br>`items = random.shuffle(items)` | `items` becomes `None`, because `shuffle` returns `None`. |
| When might shuffling be useful? | Creating a random order for a deck, turn sequence, maze candidates, or spawn schedule. |

---

## Part 8.7 - Randomness and Repetition

Randomness becomes more useful when combined with loops and functions.

| Question | Answer |
| :--- | :--- |
| What does this create?<br><br>`rolls = [random.randint(1, 6) for _ in range(5)]` | A list of five random integers from `1` through `6`. |
| Can values repeat in `rolls`? | Yes. Each die roll is independent. |
| What does this create?<br><br>`items = [random.choice(['coin', 'potion']) for _ in range(4)]` | A list of four randomly selected strings. |
| Can the list contain four `'coin'` values? | Yes. `choice` selects with replacement. |
| What does this loop count?<br><br>`successes = 0`<br>`for _ in range(100):`<br>&emsp;`if random.random() < 0.25:`<br>&emsp;&emsp;`successes += 1` | The number of successful events in one hundred trials. |
| Will `successes` always equal `25`? | No. It should be near `25` over many trials, but one run can vary. |
| What does repeated sampling reveal? | Probability describes long-run behavior, not a guaranteed result from one small experiment. |

### Fundamental Questions About Random Repetition

| Question | Answer |
| :--- | :--- |
| What does a loop provide? | Repeated opportunities for random choice. |
| Does repetition make every result predictable? | No. It makes the distribution of results easier to observe. |
| Why might a small sample be misleading? | Random variation can make a short run look unusual. |

---

## Part 8.8 - Reproducible Randomness

Python's ordinary random generator is pseudo-random. A seed determines the sequence of values it will produce.

| Question | Answer |
| :--- | :--- |
| True or false: a random sequence can be reproduced exactly if the same seed and operations are used. | **True.** |
| What does `random.seed(7)` do? | It initializes the random generator to a repeatable state. |
| What happens when the same seed is used before the same sequence of calls? | The same random values are produced in the same order. |
| Does setting a seed make the values non-random in the mathematical sense? | It makes the process deterministic and reproducible. It does not make the values less useful for ordinary game variation. |
| Why is reproducibility useful when debugging? | A bug involving a random layout can be recreated instead of disappearing on the next run. |
| Why might you avoid resetting the seed before every individual choice? | Repeatedly resetting it can produce the same patterns instead of allowing the generator to advance. |

### Fundamental Questions About Reproducible Variation

| Question | Answer |
| :--- | :--- |
| Is “random” the same as “impossible to repeat”? | No. Pseudo-random processes can be repeated from the same starting state. |
| What should be recorded when saving a reproducible experiment? | The seed and the sequence of random operations. |
| Can changing one earlier random call change every later result? | Yes. The generator advances, so the later sequence depends on the earlier calls. |

> ### Law of Reproducible Variation
> A seeded random sequence is deterministic and repeatable — “random” does not mean “impossible to reproduce.”

---

## Part 8.9 - Weighted Choices

Not every outcome needs to be equally likely.

```python
items = ['coin', 'potion', 'rare_gem']
weights = [70, 25, 5]
```

| Question | Answer |
| :--- | :--- |
| True or false: the three items above should be equally likely if their weights are different. | **False.** The weights express different relative chances. |
| Which item should appear most often? | `'coin'`, because it has the greatest weight. |
| Which item should appear least often? | `'rare_gem'`, because it has the smallest weight. |
| What does `random.choices(items, weights=weights, k=1)` return? | A one-element list containing a weighted choice. |
| What does `random.choices(items, weights=weights, k=5)` return? | A five-element list of weighted choices. Values may repeat. |
| Do the weights need to add up to `100`? | No. Only their relative sizes matter. |
| What happens if all weights are zero? | Python raises a `ValueError` because no outcome has a positive chance. |

### Fundamental Questions About Relative Weight

| Question | Answer |
| :--- | :--- |
| What does a weight represent? | Relative likelihood, not necessarily a percentage. |
| Is `[70, 25, 5]` equivalent in relative terms to `[14, 5, 1]`? | Yes. Each value was divided by `5`. |
| Does one weighted sample guarantee the most likely result? | No. It only makes that result more likely over many trials. |
| Why are weighted choices useful in games? | They allow common, uncommon, and rare outcomes without writing separate probability branches manually. |

---

## Part 8.10 - Randomness with Constraints

Sometimes a random choice must satisfy a rule.

| Question | Answer |
| :--- | :--- |
| True or false: choosing a random point guarantees that the point is valid for the game world. | **False.** A random point may be outside the map, inside an obstacle, or too close to another object. |
| What does this function do?<br><br>`def random_point(width, height):`<br>&emsp;`x = random.randrange(width)`<br>&emsp;`y = random.randrange(height)`<br>&emsp;`return (x, y)` | It generates a random coordinate inside a rectangular grid. |
| What coordinate values can `random_point(4, 3)` produce? | `x` can be `0` through `3`; `y` can be `0` through `2`. |
| How could you reject a forbidden point?<br><br>`while point in blocked:` | Keep generating candidates until the candidate is not blocked. |
| What must be true for rejection sampling to finish? | A valid result must be possible, and the process must continue generating candidates. |
| What is a danger of an impossible constraint? | The loop may continue forever if no valid candidate exists. |

### Fundamental Questions About Constrained Generation

| Question | Answer |
| :--- | :--- |
| What is a candidate? | A generated possibility that has not yet been accepted. |
| What is a constraint? | A rule that determines whether a candidate is acceptable. |
| What happens to a rejected candidate? | It is discarded, and another candidate is generated. |
| What must be considered before writing a rejection loop? | Whether a valid candidate exists and whether the loop has a reasonable stopping strategy. |

> ### Law of Constrained Generation
> Generate a candidate, then test it against a rule; reject and try again rather than trying to encode every constraint directly into the random call.

---

## Part 8.11 - Generated Structures

Randomness becomes a generator when it fills a structure with varied values.

| Question | Answer |
| :--- | :--- |
| What does this produce?<br><br>`terrain = [random.choice(['grass', 'water', 'rock']) for _ in range(8)]` | A list of eight randomly selected terrain labels. |
| Can `terrain` contain the same label several times? | Yes. Each choice is made with replacement. |
| What does this produce?<br><br>`grid = [[random.randint(0, 1) for x in range(4)] for y in range(3)]` | A three-row by four-column grid containing random zeroes and ones. |
| What does each inner comprehension create? | One row. |
| What does the outer comprehension create? | The list of rows. |
| What does this dictionary represent?<br><br>`loot = {name: random.randint(1, 10) for name in ['coin', 'gem', 'key']}` | A dictionary assigning a random integer value to each named item. |
| Are the values in `loot` guaranteed to be different? | No. Each value is generated independently. |
| How could a function make this generation reusable? | Give the size, choices, or seed as parameters and return the generated structure. |

---

## Part 8.12 - Predict the Result

Some questions have exact answers. Others ask what is guaranteed, what is possible, or what cannot happen.

| Question | Answer |
| :--- | :--- |
| What values are possible?<br><br>`random.seed(4)`<br>`result = random.randrange(3)` | `0`, `1`, or `2`. The exact seeded result can be calculated by running Python, but `3` is impossible. |
| What is guaranteed about `result`?<br><br>`result = random.choice(['a', 'b', 'c'])` | It must be one of `'a'`, `'b'`, or `'c'`. |
| What is guaranteed about `rolls`?<br><br>`rolls = [random.randint(1, 6) for _ in range(10)]` | It has length `10`, and every element is an integer from `1` through `6`. |
| Can `rolls` contain ten identical values? | Yes. Repeated values are possible. |
| What is guaranteed about `sample`?<br><br>`sample = random.sample([1, 2, 3, 4], 2)` | It has length `2`, and its two elements come from the source without selecting the same position twice. |
| What happens to `items`?<br><br>`items = ['a', 'b', 'c']`<br>`result = random.shuffle(items)` | `items` is rearranged; `result` is `None`. |
| What is the possible range of `x` and `y`?<br><br>`x = random.randrange(5)`<br>`y = random.randrange(2, 7)` | `x` is `0` through `4`; `y` is `2` through `6`. |
| What does `random.seed(9)` guarantee? | Repeating the same random operations after the same seed produces the same sequence. |
| What is the type of `loot`?<br><br>`loot = {name: random.randint(1, 10) for name in ['coin', 'gem']}` | `dict`, with string keys and integer values. |

---

## Chapter Summary

| Question | Answer |
| :--- | :--- |
| What does `random.random()` produce? | A float from `0.0` up to but not including `1.0`. |
| What does `random.randint(a, b)` produce? | An integer from `a` through `b`, including both endpoints. |
| What does `random.randrange(...)` follow? | The same start, stop, and step rules as `range`, including an excluded stop value. |
| What does `random.choice` do? | Selects one existing element, with replacement. |
| What does `random.sample` do? | Selects several positions without replacement. |
| What does `random.shuffle` do? | Rearranges a mutable sequence in place and returns `None`. |
| What does `random.seed` do? | Makes a sequence of pseudo-random operations reproducible. |
| What do weights control? | Relative likelihood in weighted choices. |
| What is constrained generation? | Generating candidates and accepting only those that satisfy a rule. |
| What is the central idea? | Randomness creates variation, while code supplies the boundaries, structure, and rules that make the variation useful. |

---

**==> Randomness supplies surprise. The structure of the program gives that surprise a place to belong. <==**
