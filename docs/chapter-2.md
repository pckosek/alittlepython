# Chapter 2: Access, Membership, and Change

In Chapter 1, we asked:

> What kind of container is this?

Now we ask a more useful question:

> How do I get something out of a container, and how do I change the container afterward?

There are three basic ways to interact with a structure:

1. retrieve a value;
2. test whether a value is present; and
3. change the structure.

The exact operation depends on the kind of container. Lists and tuples use positions. Dictionaries use keys. Sets care about membership and uniqueness rather than position.

---

## Part 1 - Asking a Container for a Value

| Question | Answer |
| :--- | :--- |
| What is `items[0]` when `items = ['sword', 'shield', 'potion']`? | `'sword'`. Lists are accessed by index. |
| What is `items[2]`? | `'potion'`. Index `2` is the third position. |
| What is `items[-1]`? | `'potion'`. Negative index `-1` refers to the last element. |
| What is `point[0]` when `point = (3, 4)`? | `3`. Tuples are also accessed by index. |
| What is `point[1]`? | `4`. |
| What is `player['health']` when `player = {'health': 100, 'coins': 25}`? | `100`. Dictionaries are accessed by key. |
| What is `player['coins']`? | `25`. |
| What is `player[0]`? | No value. Python raises a `KeyError` because `0` is not a key in the dictionary. A dictionary does not use list-style positions. |
| What is `grid[(2, 3)]` when `grid = {(2, 3): 'treasure'}`? | `'treasure'`. A tuple can serve as a dictionary key. |

### Fundamental Questions About Access

| Question | Answer |
| :--- | :--- |
| What does a list use to find a value? | A numeric index. |
| What does a tuple use to find a value? | A numeric index. |
| What does a dictionary use to find a value? | A key. |
| What does a set use to find a value? | A membership test such as `value in set`. Sets do not provide positional indexing. |
| What happens when you request a list index that does not exist? | Python raises an `IndexError`. |
| What happens when you request a dictionary key that does not exist? | Python raises a `KeyError`. |

---

## Part 2 - Membership

The expression `in` asks whether a value belongs to a structure.

| Question | Answer |
| :--- | :--- |
| Is `'shield' in items` true when `items = ['sword', 'shield', 'potion']`? | **True.** `'shield'` is an element of the list. |
| Is `'helmet' in items` true? | **False.** `'helmet'` is not an element of the list. |
| Is `4 in point` true when `point = (3, 4)`? | **True.** Tuples can be searched by membership. |
| Is `(2, 3) in visited` true when `visited = {(1, 1), (2, 3)}`? | **True.** The coordinate tuple is an element of the set. |
| Is `(5, 5) in visited` true? | **False.** That coordinate has not been added to the set. |
| Is `'health' in player` true when `player = {'health': 100, 'coins': 25}`? | **True.** Membership in a dictionary checks its keys. |
| Is `100 in player` true? | **False.** `100` is a value, not a key. |
| Is `100 in player.values()` true? | **True.** `.values()` provides the dictionary's values for membership testing. |

### Fundamental Questions About Membership

| Question | Answer |
| :--- | :--- |
| What does `x in list` search? | The elements of the list. |
| What does `x in tuple` search? | The elements of the tuple. |
| What does `x in set` search? | The elements of the set. |
| What does `x in dictionary` search? | The keys of the dictionary. |
| How can you search dictionary values? | Use `x in dictionary.values()`. |

---

## Part 3 - Adding to a List

A list is mutable. It can become larger after it is created.

| Question | Answer |
| :--- | :--- |
| What is `items` after `items = []` followed by `items.append('sword')`? | `['sword']`. `append` adds one element to the end of the list. |
| What is `items` after `items.append('shield')`? | `['sword', 'shield']`. |
| What is `items` after `items.append(['potion', 'map'])`? | `['sword', 'shield', ['potion', 'map']]`. The appended list becomes one element. |
| What is the difference between `items.append(['potion', 'map'])` and `items += ['potion', 'map']`? | The first adds one list as one element. The second adds `'potion'` and `'map'` as two separate elements. |
| What is `items` after `items.append('sword')` when `items = ['sword']`? | `['sword', 'sword']`. Lists allow duplicates. |
| What does `append` return? | `None`. It changes the list in place rather than returning a new list. |

### Fundamental Questions About Appending

| Question | Answer |
| :--- | :--- |
| What does `items.append(value)` do? | It places `value` at the end of `items`. |
| Does `append` add exactly one element? | Yes. Even if that element is itself a list, the list is added as one element. |
| Does `append` change the original list? | Yes. It mutates the list in place. |
| What is the common mistake in `items = items.append('sword')`? | `append` returns `None`, so this assignment replaces `items` with `None`. Use `items.append('sword')` by itself. |

> ### Law of Appending
> `append` always adds exactly one element to the end of a list, mutates that list in place, and returns `None`.

---

## Part 4 - Removing from a List

Lists can also become smaller.

| Question | Answer |
| :--- | :--- |
| What is `items` after `items.remove('shield')` when `items = ['sword', 'shield', 'potion']`? | `['sword', 'potion']`. `remove` removes the first matching value. |
| What happens if `items.remove('helmet')` and `'helmet'` is not in the list? | Python raises a `ValueError`. There is no matching value to remove. |
| What is `removed` after `removed = items.pop()` when `items = ['sword', 'shield', 'potion']`? | `'potion'`. `pop()` removes and returns the last element. |
| What is `items` after that `pop()`? | `['sword', 'shield']`. |
| What is `removed` after `removed = items.pop(0)`? | `'sword'`. `pop(0)` removes and returns the element at index `0`. |
| What is the difference between `remove('shield')` and `pop(1)`? | `remove` identifies a value. `pop` identifies an index and returns the removed value. |

### Fundamental Questions About Removal

| Question | Answer |
| :--- | :--- |
| Which operation removes a matching value? | `remove(value)`. |
| Which operation removes an element by index? | `pop(index)`. |
| Which operation removes the last element by default? | `pop()`. |
| What happens to the list after a successful removal? | Its size decreases by one. |

---

## Part 5 - Changing a Value in a List

Adding and removing change the size of a list. Index assignment changes a value without changing the size.

| Question | Answer |
| :--- | :--- |
| What is `items` after `items[0] = 'axe'` when `items = ['sword', 'shield']`? | `['axe', 'shield']`. |
| Does `items[0] = 'axe'` make the list longer? | No. It replaces one existing element. |
| What happens after `items[2] = 'potion'` when `items = ['sword', 'shield']`? | Python raises an `IndexError`. There is no index `2`; assignment does not automatically append. |
| How would you add `'potion'` to the end of that list? | Use `items.append('potion')`. |
| What happens after `items[0][0] = 'A'` when `items = [['a', 'b'], ['c', 'd']]`? | The list becomes `[['A', 'b'], ['c', 'd']]`. The first index selects the inner list and the second selects its first element. |

---

## Part 6 - Adding and Updating Dictionary Entries

A dictionary changes by assigning a value to a key.

| Question | Answer |
| :--- | :--- |
| What is `player` after `player = {}` followed by `player['health'] = 100`? | `{'health': 100}`. A new key-value pair has been added. |
| What is `player` after `player['coins'] = 25`? | `{'health': 100, 'coins': 25}`. |
| What is `player['health']` after `player['health'] = 75`? | `75`. Assigning to an existing key replaces its value. |
| Does assigning to an existing key create a second entry? | No. Dictionary keys are unique. |
| What is `grid` after `grid = {(0, 0): 'wall'}` followed by `grid[(0, 1)] = 'passage'`? | `{(0, 0): 'wall', (0, 1): 'passage'}`. |
| What is `grid[(0, 0)]` after `grid[(0, 0)] = 'passage'`? | `'passage'`. The old value was overwritten. |
| Can a dictionary value be a list? | Yes. For example, `player['inventory'] = ['sword', 'potion']`. |
| Can a dictionary value be another dictionary? | Yes. Dictionaries may be nested. |

### Fundamental Questions About Dictionary Assignment

| Question | Answer |
| :--- | :--- |
| What happens when you assign a value to a new key? | The dictionary grows by one entry. |
| What happens when you assign a value to an existing key? | The old value is replaced. |
| What determines whether a dictionary grows? | Whether the assigned key already exists. |
| What happens after `del player['coins']`? | The key `'coins'` and its associated value are removed. |
| What happens after `del player['score']` when `'score'` is not a key? | Python raises a `KeyError`. |

---

## Part 7 - Safe Dictionary Access

Square brackets are precise. `.get()` is forgiving.

| Question | Answer |
| :--- | :--- |
| What is `player['health']` when `player = {'health': 100}`? | `100`. |
| What is `player.get('health')`? | `100`. |
| What is `player['score']` when `'score'` is missing? | Python raises a `KeyError`. |
| What is `player.get('score')` when `'score'` is missing? | `None`. `.get()` uses `None` as its default when no default is provided. |
| What is `player.get('score', 0)` when `'score'` is missing? | `0`. |
| Which expression asks for a key that must exist? | `player['score']`. |
| Which expression supplies a fallback when a key may be missing? | `player.get('score', default_value)`. |

### Fundamental Questions About Safe Lookup

| Question | Answer |
| :--- | :--- |
| When should you use square brackets with a dictionary? | When a missing key should be treated as an error. |
| When should you use `.get()`? | When a missing key has a reasonable fallback value. |
| Does `.get()` add the missing key to the dictionary? | No. It only returns a value. |

> ### Law of Safe Lookup
> Use `d[key]` when a missing key is a mistake, and `d.get(key, default)` when a missing key is an ordinary possibility.

---

## Part 8 - Adding and Removing Set Members

A set has no positions. Its basic operations ask whether a value belongs and then add or remove that value.

| Question | Answer |
| :--- | :--- |
| What is `visited` after `visited = set()` followed by `visited.add((0, 0))`? | `{(0, 0)}`. |
| What is `visited` after `visited.add((1, 0))`? | `{(0, 0), (1, 0)}`. |
| What happens after `visited.add((0, 0))` again? | Nothing changes. The set already contains `(0, 0)`. |
| What is `visited` after `visited.remove((1, 0))`? | `{(0, 0)}`. |
| What happens when `visited.remove((5, 5))` and `(5, 5)` is missing? | Python raises a `KeyError`. |
| What happens when `visited.discard((5, 5))` and `(5, 5)` is missing? | Nothing happens. `discard` safely ignores a missing value. |
| What is the size of a set after adding the same value three times? | It increases by only one. A set stores one copy of each value. |

### Fundamental Questions About Set Membership

| Question | Answer |
| :--- | :--- |
| What does `set.add(value)` do? | It adds `value` if it is not already present. |
| What does `set.remove(value)` do? | It removes `value`, but raises an error if the value is missing. |
| What does `set.discard(value)` do? | It removes `value` if present and otherwise does nothing. |
| Why is a set useful for visited locations? | A location can be visited many times, but the set records it only once. |

> ### Law of Set Membership
> `.add()` and `.discard()` never raise an error on a duplicate or a missing value; `.remove()` raises `KeyError` when the value is absent.

---

## Part 9 - Tuples as Fixed Values

Tuples are different because they can be read but not changed in place.

| Question | Answer |
| :--- | :--- |
| What is `x, y` after `x, y = (3, 4)`? | `x` is `3` and `y` is `4`. |
| What is `point` after `x, y = point` when `point = (3, 4)`? | `point` remains `(3, 4)`. Unpacking creates names; it does not change the tuple. |
| What is `pt + (1, 0)` when `pt = (2, 3)`? | `(2, 3, 1, 0)`. Tuple addition concatenates tuples. It does not add coordinates. |
| If `x, y = (2, 3)` and `dx, dy = (1, 0)`, what is `(x + dx, y + dy)`? | `(3, 3)`. The coordinates are added component by component. |
| Can you write `point[0] = 5` when `point = (2, 3)`? | No. Python raises a `TypeError` because tuples are immutable. |
| Why might a coordinate be represented as a tuple rather than a list? | A coordinate has a fixed number of parts and often should not be changed in place. |

### Fundamental Questions About Coordinate Offsets

| Question | Answer |
| :--- | :--- |
| What is a point such as `(x, y)`? | A fixed pair of related values. |
| What is an offset such as `(dx, dy)`? | A pair of changes to apply to a point. |
| How do you move a point by an offset? | Unpack both tuples and calculate `(x + dx, y + dy)`. |
| Does tuple addition calculate a spatial offset? | No. It concatenates the tuples. |

---

## Part 10 - Predict the Result

Do not run these immediately. Predict the final value and the data type.

| Question | Answer |
| :--- | :--- |
| What is `items`?<br><br>`items = []`<br>`items.append(3)`<br>`items.append(3)`<br>`items.pop()` | `[3]`, a list. The second `3` is appended and then removed. |
| What is `player`?<br><br>`player = {}`<br>`player['score'] = 5`<br>`player['score'] = 7` | `{'score': 7}`, a dictionary. The second assignment overwrites the first value. |
| What is `visited`?<br><br>`visited = set()`<br>`visited.add(1)`<br>`visited.add(1)`<br>`visited.add(2)` | `{1, 2}`, a set. The duplicate `1` is stored only once. |
| What is `point`?<br><br>`point = (2, 3)`<br>`x, y = point`<br>`point = (x + 1, y)` | `(3, 3)`, a tuple. A new tuple is assigned to `point`; the original tuple was not mutated. |
| What is `items`?<br><br>`items = ['a', 'b', 'c']`<br>`removed = items.pop(1)` | `items` is `['a', 'c']`, and `removed` is `'b'`. |
| What is `value`?<br><br>`player = {'health': 100}`<br>`value = player.get('coins', 0)` | `0`, an integer. The missing key does not cause an error because `.get()` supplies a default. |

---

## Chapter Summary

| Container | How you access it | How you change it |
| :--- | :--- | :--- |
| List | By index | `append`, `remove`, `pop`, or index assignment |
| Tuple | By index | You do not change it in place; create a new tuple instead |
| Set | By membership | `add`, `remove`, or `discard` |
| Dictionary | By key | Assign a key, overwrite a key, or delete a key |

The important question is now:

> What operation matches the kind of change I want to make?

---

**==> You are no longer only identifying the shape of data. You are learning how data responds when you touch it. <==**
