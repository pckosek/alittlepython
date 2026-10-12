# Chapter 10: Sets, Membership, and Regions

In Chapter 9, a coordinate gave us a location in a world.

We could store a value at that location. We could find its neighbors. We could change its state.

Now we ask:

> How can a program remember a group of locations?

Perhaps some coordinates are blocked.

Perhaps some coordinates have already been visited.

Perhaps some coordinates belong to a region, a team, or a collection of targets.

A list can store these coordinates. A dictionary can associate each coordinate with a value.

But sometimes the question is simpler:

> Is this thing a member of that group?

That is the question a set is designed to answer.

---

## Part 10.1 - A Set of Values

A set is a collection of distinct values.

```python
colors = {'red', 'blue', 'green'}
```

| Question | Answer |
| :--- | :--- |
| True or false: `colors` contains three values. | **True.** The set contains `'red'`, `'blue'`, and `'green'`. |
| Is a set ordered like a list? | No. A set represents membership, not a dependable left-to-right order. |
| Can a set contain the same value twice? | No. A set keeps only one copy of each value. |
| What happens here?<br><br>`colors = {'red', 'blue', 'red'}` | The set contains only `'red'` and `'blue'`. The repeated `'red'` does not create a second member. |
| What is the data type of `colors`? | `set`. |
| What idea does a set emphasize? | Whether a value belongs to a collection. |

### Fundamental Questions About Set Membership

| Question | Answer |
| :--- | :--- |
| What does a list emphasize? | Values in a particular sequence. |
| What does a dictionary emphasize? | A relationship between keys and values. |
| What does a set emphasize? | Distinct membership. |
| Does the order of values matter when comparing sets? | No. Two sets with the same members are equal even if they were written in a different order. |
| What is the central question for a set? | “Is this value in the collection?” |

---

## Part 10.2 - Membership with `in`

The operator `in` asks whether a value belongs to a collection.

```python
blocked = {'rock', 'water'}
```

| Question | Answer |
| :--- | :--- |
| True or false: `'rock' in blocked` is `True`. | **True.** `'rock'` is a member of the set. |
| Is `'grass' in blocked` true? | No. `'grass'` is not a member of the set. |
| What is the type of `'rock' in blocked`? | `bool`. Membership questions produce `True` or `False`. |
| What does `not in` ask? | Whether a value is absent from the collection. |
| What is `'grass' not in blocked`? | `True`. |
| How could a movement rule use this set? | It could reject a destination if the destination's terrain is in `blocked`. |

### Fundamental Questions About Membership Tests

| Question | Answer |
| :--- | :--- |
| What does `value in collection` produce? | A Boolean answer. |
| Can the result of `in` be used in an `if` statement? | Yes. |
| What does this mean?<br><br>`if terrain in blocked:` | If the current terrain belongs to the blocked set, run the indented code. |
| What does this mean?<br><br>`if point not in visited:` | If the coordinate has not yet been visited, run the indented code. |
| Is membership the same question as indexing? | No. Indexing asks where a value is stored. Membership asks whether a value belongs. |

> ### Law of Membership Tests
> `in` asks whether a value belongs to a collection and always answers with a Boolean — it never tells you where.

---

## Part 10.3 - Coordinates as Set Members

In Chapter 9, a coordinate was represented by a tuple.

A tuple can also be a member of a set.

```python
blocked = {
    (1, 0),
    (2, 1),
    (3, 2)
}
```

| Question | Answer |
| :--- | :--- |
| True or false: `(1, 0) in blocked` is `True`. | **True.** The tuple `(1, 0)` is one of the set's members. |
| Is `(0, 1) in blocked` true? | No. That coordinate is not in the set. |
| What does `(2, 1) in blocked` ask? | Whether the coordinate `(2, 1)` is one of the blocked locations. |
| What is the data type of each member of `blocked`? | `tuple`. |
| Does the set need to store every coordinate in a `4 x 3` world? | No. It stores only the coordinates that are blocked. |
| What kind of representation is this? | A sparse representation of blocked locations. |

### Fundamental Questions About Coordinate Membership

| Question | Answer |
| :--- | :--- |
| What does a tuple such as `(2, 1)` represent? | One coordinate: `x = 2`, `y = 1`. |
| What does a set such as `blocked` represent? | A group of coordinates with a shared meaning. |
| What does `(x, y) in blocked` ask? | Whether the coordinate belongs to that group. |
| Why is this useful for a gridworld? | The program can ask whether a location is blocked without scanning every cell. |
| Does the set itself tell us what is at every other coordinate? | No. It tells us only which coordinates belong to the blocked group. |

> ### Law of Coordinate Membership
> Checking `point in blocked_set` costs the same no matter how large the world grows, while checking `point in blocked_list` gets slower as the list grows — this is why sets, not lists, hold blocked and visited coordinates.

---

## Part 10.4 - Adding a Member

The method `.add()` places one value into a set.

```python
visited = set()
visited.add((0, 0))
```

| Question | Answer |
| :--- | :--- |
| True or false: `set()` creates an empty set. | **True.** It creates a set with no members. |
| What does `visited.add((0, 0))` do? | It records `(0, 0)` as a member of `visited`. |
| What is `(0, 0) in visited` afterward? | `True`. |
| What happens if `.add((0, 0))` is called again? | The set remains unchanged. A set does not create duplicate members. |
| What happens after `visited.add((1, 0))`? | Both `(0, 0)` and `(1, 0)` belong to `visited`. |
| What does `.add()` return? | `None`. It changes the set in place. |

### Fundamental Questions About Adding to a Set

| Question | Answer |
| :--- | :--- |
| Does `.add()` replace the set? | No. It changes the existing set. |
| Does `.add()` return the updated set? | No. It returns `None`. |
| What is the safe pattern?<br><br>`visited.add(point)` | Add the value without assigning the result back to `visited`. |
| What is the common mistake?<br><br>`visited = visited.add(point)` | `visited` becomes `None`, because `.add()` does not return the set. |
| Why is adding to a visited set useful? | A program can remember where it has already been. |

---

## Part 10.5 - Removing a Member

A set can also forget a member.

```python
targets = {(0, 0), (2, 1)}
targets.remove((0, 0))
```

| Question | Answer |
| :--- | :--- |
| What is in `targets` after the code runs? | Only `(2, 1)`. |
| What happens if `.remove()` is used on a missing member? | Python raises a `KeyError`. |
| What does `.discard()` do differently? | It removes a member if present and does nothing if the member is absent. |
| Which method is safer when absence is ordinary?<br><br>`targets.remove(point)` or `targets.discard(point)` | `.discard(point)`, because a missing member is not an error. |
| What is the result of `targets.discard((9, 9))`? | `None`, with the set left unchanged. |
| Why might a game remove a coordinate from a target set? | The target may have been collected, reached, or completed. |

### Fundamental Questions About Set Mutation

| Question | Answer |
| :--- | :--- |
| What does mutation mean here? | Changing an existing set. |
| Do `.add()`, `.remove()`, and `.discard()` return a new set? | No. They change the set in place and return `None`. |
| When should `.remove()` be useful? | When the member is expected to exist and its absence indicates a mistake. |
| When should `.discard()` be useful? | When removing an absent member is harmless. |

---

## Part 10.6 - A Set of Visited Locations

The most important gridworld use of a set is often memory.

```python
visited = set()

point = (0, 0)
if point not in visited:
    visited.add(point)
```

| Question | Answer |
| :--- | :--- |
| True or false: the first time this code runs, `(0, 0)` is not in `visited`. | **True.** The set begins empty. |
| What does the `if` statement protect? | It ensures that the body runs only for an unvisited point. |
| What happens inside the body? | `(0, 0)` is added to `visited`. |
| What happens if the same code runs again? | The condition is false, because `(0, 0)` is already a member. |
| Why is this better than a list for the membership question? | The code expresses the purpose directly: remember a collection of distinct visited coordinates. |
| What does `visited` represent? | The set of locations the process has already encountered. |

### Fundamental Questions About Remembered Locations

| Question | Answer |
| :--- | :--- |
| What does the world grid store? | Cell states. |
| What does `visited` store? | A history of locations encountered by a process. |
| Are the cell state and visited status the same idea? | No. A cell may be open but unvisited, or blocked but previously examined. |
| Can a location belong to both `blocked` and `visited`? | Yes. The sets answer different questions. |
| Why keep those ideas in separate structures? | A location's world state and a process's history can change independently. |

---

## Part 10.7 - Set Operations

Sets can be compared and combined.

```python
red_team = {'A', 'B', 'C'}
blue_team = {'C', 'D'}
```

| Question | Answer |
| :--- | :--- |
| True or false: `'C'` belongs to both teams. | **True.** `'C'` is a member of each set. |
| What does `red_team | blue_team` mean? | The union: every member in either set. |
| What is `red_team | blue_team`? | A set containing `'A'`, `'B'`, `'C'`, and `'D'`. |
| What does `red_team & blue_team` mean? | The intersection: members shared by both sets. |
| What is `red_team & blue_team`? | `{'C'}`. |
| What does `red_team - blue_team` mean? | Members in `red_team` that are not in `blue_team`. |
| What is `red_team - blue_team`? | A set containing `'A'` and `'B'`. |

### Fundamental Questions About Set Relationships

| Question | Answer |
| :--- | :--- |
| What does union ask? | “What belongs to at least one of these groups?” |
| What does intersection ask? | “What belongs to both groups?” |
| What does difference ask? | “What belongs to the first group but not the second?” |
| How could intersection help with a world? | It could find locations that are both reachable and targets. |
| How could difference help with a world? | It could find open locations that have not yet been visited. |
| Does a set operation necessarily mutate either original set? | No. The operators produce a new set. |

---

## Part 10.8 - Sets and Gridworld Rules

Now we can combine several spatial ideas.

```python
blocked = {(1, 0), (2, 1), (3, 2)}
visited = {(0, 0), (0, 1)}
point = (1, 1)
```

| Question | Answer |
| :--- | :--- |
| Is `point in blocked` true? | No. `(1, 1)` is not blocked. |
| Is `point in visited` true? | No. `(1, 1)` has not been visited. |
| What condition means “the point is open and new”?<br><br>`point not in blocked and point not in visited` | Both membership tests must be true. |
| What condition means “the point is either blocked or already visited”? | `point in blocked or point in visited`. |
| What happens if a point is both blocked and visited? | The two membership questions can both be true; neither set erases the other. |
| What does this show? | A rule can combine several independent collections of locations. |

### Fundamental Questions About Spatial Predicates

| Question | Answer |
| :--- | :--- |
| What is a predicate? | A function or expression that answers a true-or-false question. |
| Is `point not in blocked` a predicate? | Yes. |
| What does a movement predicate need to decide? | Whether a destination is in bounds, open, and perhaps not already visited. |
| Why are sets useful inside predicates? | Membership questions are concise and match the rule being expressed. |
| What larger pattern is appearing? | Coordinates provide the objects; sets provide named groups; predicates provide rules. |

---

## Part 10.9 - Set Comprehensions

A set comprehension builds a set from a rule.

```python
even_squares = {number * number for number in range(6) if number % 2 == 0}
```

| Question | Answer |
| :--- | :--- |
| True or false: this produces a set. | **True.** Curly braces with a value expression and a `for` clause create a set comprehension. |
| Which values does `number` take? | `0`, `1`, `2`, `3`, `4`, and `5`. |
| Which values pass the condition? | `0`, `2`, and `4`. |
| What values are added to the set? | Their squares: `0`, `4`, and `16`. |
| What is `even_squares`? | A set containing `0`, `4`, and `16`. |
| Why might repeated results disappear? | A set stores each resulting value only once. |

The same idea can describe coordinates.

```python
top_row = {(x, 0) for x in range(4)}
```

| Question | Answer |
| :--- | :--- |
| What coordinates does `x` produce? | `(0, 0)`, `(1, 0)`, `(2, 0)`, and `(3, 0)`. |
| What is `top_row`? | A set containing the four coordinates in the top row of a `4 x 3` world. |
| What does the expression vary? | The `x` coordinate. |
| What does the expression hold constant? | The `y` coordinate, which remains `0`. |
| How could we describe the bottom row? | `{(x, 2) for x in range(4)}`. |

### Fundamental Questions About Set Generation

| Question | Answer |
| :--- | :--- |
| What does a set comprehension combine? | A value expression, a loop, and optionally a condition. |
| What does the loop provide? | Candidate values. |
| What does the condition provide? | A filter that accepts some candidates and rejects others. |
| What does the expression provide? | The member to place into the resulting set. |
| How is a set comprehension different from a list comprehension? | It produces a set, so duplicate results collapse and membership is the central idea. |

---

## Part 10.10 - The Frontier of a Region

Suppose a process has already visited a group of cells.

Which nearby cells have not yet been visited?

```python
visited = {(0, 0), (1, 0), (0, 1)}
nearby = {(1, 0), (0, 1), (1, 1), (0, 0)}
```

| Question | Answer |
| :--- | :--- |
| True or false: `nearby` contains locations already in `visited`. | **True.** `(1, 0)`, `(0, 1)`, and `(0, 0)` occur in both sets. |
| What does `nearby - visited` produce? | The nearby locations that have not yet been visited. |
| What is `nearby - visited`? | `{(1, 1)}`. |
| What does `visited | nearby` represent? | Every location known to either collection. |
| What might we call the unvisited nearby locations? | A frontier: the edge between what is known and what has not yet been explored. |
| Does finding the frontier complete a search algorithm? | No. It gives us a useful next collection of candidates. |

### Fundamental Questions About the Frontier

| Question | Answer |
| :--- | :--- |
| What is a region? | A group of locations treated as belonging together. |
| What is a frontier? | A group of candidate locations adjacent to, or near, a known region but not yet included. |
| Why subtract `visited` from `nearby`? | To remove locations that have already been handled. |
| What would a later algorithm do with the frontier? | It could examine, filter, and expand those candidates. |

> ### Law of the Frontier
> A frontier is `nearby - visited`: the candidates adjacent to a known region that have not yet been absorbed into it — this is the seed of every flood fill, maze carve, and region-growing algorithm to come.

| What new possibility does this create? | A program can grow a region step by step instead of inspecting every coordinate at once. |

---

## Part 10.11 - Sets Beside Lists and Dictionaries

The same world can use several structures at once.

```python
grid = [
    0, 1, 0, 0,
    2, 0, 1, 0,
    0, 0, 1, 2
]

blocked = {(0, 1), (3, 2)}
visited = {(0, 0)}
```

| Question | Answer |
| :--- | :--- |
| What does `grid` store? | The state of every cell in flat storage. |
| What does `blocked` store? | A sparse group of coordinates that should not be entered. |
| What does `visited` store? | A sparse group of coordinates already encountered by a process. |
| Can `grid` tell us whether `(3, 2)` is blocked? | Only if the grid's cell values have been defined to encode that meaning. The separate set makes the rule explicit. |
| Can `blocked` tell us the state value at `(3, 2)`? | No. It tells us membership in a category, not the cell's stored value. |
| Why might a program keep all three structures? | Each structure answers a different question. |

### Fundamental Questions About Multiple Representations

| Question | Answer |
| :--- | :--- |
| Must one data structure contain every fact about a world? | No. Different structures can represent different aspects of the same world. |
| What does a flat list provide? | Dense storage and state values. |
| What does a set provide? | Distinct membership in a named group. |
| What does a dictionary provide? | A mapping from keys to associated values. |
| What is the danger of using several structures? | They can disagree if the program changes one but forgets the others. |
| What is the benefit? | Each rule can be expressed using the representation that fits it best. |

> ### Law of Multiple Representations
> No single structure has to describe an entire world — a flat list can hold dense state while sets hold sparse membership, as long as the program keeps them honest with each other.

---

## Part 10.12 - Predict the Result

Do not run these immediately. Predict the final value and the data type.

| Question | Answer |
| :--- | :--- |
| What is `result`?<br><br>`blocked = {(1, 0), (2, 1)}`<br>`result = (1, 0) in blocked` | `True`, a Boolean. |
| What is `result`?<br><br>`values = {1, 1, 2, 3, 3}`<br>`result = len(values)` | `3`, an integer. Duplicate values occur only once. |
| What is `visited`?<br><br>`visited = set()`<br>`visited.add((0, 0))`<br>`visited.add((0, 0))` | `{(0, 0)}`, a set with one member. |
| What is `result`?<br><br>`red = {'A', 'B', 'C'}`<br>`blue = {'C', 'D'}`<br>`result = red & blue` | `{'C'}`, a set. |
| What is `result`?<br><br>`red = {'A', 'B', 'C'}`<br>`blue = {'C', 'D'}`<br>`result = red - blue` | A set containing `'A'` and `'B'`. |
| What is `result`?<br><br>`blocked = {(1, 0), (2, 1)}`<br>`point = (0, 0)`<br>`result = point not in blocked` | `True`, a Boolean. |
| What is `top_row`?<br><br>`top_row = {(x, 0) for x in range(4)}` | A set containing `(0, 0)`, `(1, 0)`, `(2, 0)`, and `(3, 0)`. |
| What is `result`?<br><br>`visited = {(0, 0), (1, 0)}`<br>`nearby = {(1, 0), (1, 1)}`<br>`result = nearby - visited` | `{(1, 1)}`, a set. |
| What is `result`?<br><br>`items = {1, 2}`<br>`result = items.add(3)` | `None`. The set becomes a set containing `1`, `2`, and `3`. |

---

## Chapter Summary

| Question | Answer |
| :--- | :--- |
| What does a set store? | Distinct values whose membership matters. |
| What does `in` ask? | Whether a value belongs to a collection. |
| How can a set represent a gridworld idea? | It can store coordinates belonging to a group such as blocked, visited, or targeted locations. |
| What does `.add()` do? | Adds a member in place and returns `None`. |
| What is the difference between `.remove()` and `.discard()`? | `.remove()` raises an error when the member is absent; `.discard()` does not. |
| What does union provide? | Members from either set. |
| What does intersection provide? | Members shared by both sets. |
| What does difference provide? | Members in one set but not the other. |
| What does a set comprehension provide? | A concise way to generate a set from a loop and optional condition. |
| What is the central idea? | A set lets a program name and manipulate a group of distinct locations without confusing membership with order or storage position. |

---

**==> A list remembers order. A dictionary remembers relationships. A set remembers belonging. <==**
