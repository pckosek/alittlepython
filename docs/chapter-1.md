# Chapter 1: Containers and Elements

Python gives us several ways to hold things.

Before we ask Python to make decisions or repeat actions, we need to become comfortable with the things Python can hold:

- lists;
- tuples;
- sets; and
- dictionaries.

The first questions are deliberately simple. The point is not to show that you already know the answer. The point is to make the answer automatic.

---

## Part 1 - Lists and Elements

| Question | Answer |
| :--- | :--- |
| Is `['turkey', 'roast', 'gravy']` a list? | **Yes.** It is a sequence of items enclosed in square brackets. |
| Is `[]` a list? | **Yes.** It is a list containing zero items. This is the **empty list**. |
| Is `'turkey'` a list? | **No.** `'turkey'` is a string. It is not enclosed in square brackets. |
| Is `[['hotdog'], 'relish']` a list? | **Yes.** A list may contain another list as one of its elements. |
| What is `lst[0]` when `lst = ['hotdog', 'pickle', 'relish']`? | `'hotdog'`. Index `0` refers to the first element. |
| What is `lst[0]` when `lst = [['hotdog'], 'relish']`? | `['hotdog']`. The first element happens to be another list. |
| What is `lst[0]` when `lst = 'hotdog'`? | `'h'`. Strings are also sequences, so their characters can be accessed by index. |
| What is `lst[0]` when `lst = []`? | No value. Python raises an `IndexError` because the empty list has no element at index `0`. |

### Fundamental Questions About Index `0`

| Question | Answer |
| :--- | :--- |
| When is `lst[0]` defined for a list? | When the list contains at least one element. |
| What does index `0` mean? | The first element of a sequence. Python begins counting sequence positions at `0`. |

> ### Law of Index Zero
> `lst[0]` is defined exactly when `lst` has at least one element, and it always names the first one.

| Question | Answer |
| :--- | :--- |
| What is `lst[1:]` when `lst = ['hotdog', 'pickle', 'relish']`? | `['pickle', 'relish']`. The slice begins at index `1` and continues to the end. |
| What is `lst[1:]` when `lst = ['hamburger']`? | `[]`. There are no elements after index `0`. |
| What is `lst[1:]` when `lst = []`? | `[]`. Slicing an empty list produces another empty list. |
| What is `lst[0]` where `lst` is the result of `['bacon', 'lettuce', 'tomato'][1:]`? | `'lettuce'`. The slice produces `['lettuce', 'tomato']`, whose first element is `'lettuce'`. |

### Fundamental Questions About Slicing

| Question | Answer |
| :--- | :--- |
| What does `lst[1:]` do? | It creates a new list containing the elements from index `1` through the end. |
| Is the stop position included in a slice such as `lst[1:3]`? | No. A slice includes the start position but stops before the stop position. |
| What is `lst[1:3]` when `lst = ['a', 'b', 'c', 'd']`? | `['b', 'c']`. Indices `1` and `2` are included; index `3` is not. |
| What is `lst[:2]` when `lst = ['a', 'b', 'c', 'd']`? | `['a', 'b']`. An omitted start defaults to `0`. |
| What is `lst[2:]` when `lst = ['a', 'b', 'c', 'd']`? | `['c', 'd']`. An omitted stop defaults to the end. |
| What is `lst[:]` when `lst = ['a', 'b']`? | `['a', 'b']`. The slice copies the outer list. |

| Question | Answer |
| :--- | :--- |
| What is `lst[-1]` when `lst = ['turkey', 'roast', 'gravy']`? | `'gravy'`. Index `-1` means the last element. |
| What is `lst[-2]` when `lst = ['turkey', 'roast', 'gravy']`? | `'roast'`. Index `-2` means the second element from the end. |
| What is `lst[-1]` when `lst = []`? | No value. Python raises an `IndexError` because the list is empty. |
| What is `lst[::-1]` when `lst = ['bacon', 'lettuce', 'tomato']`? | `['tomato', 'lettuce', 'bacon']`. A step of `-1` walks backward through the list. |

### Fundamental Questions About Backwards Indexing

| Question | Answer |
| :--- | :--- |
| What does `lst[-n]` mean? | It counts backward from the end of a non-empty sequence. |
| What does the third number in `lst[start:stop:step]` control? | The distance between the indices visited by the slice. |

---

## Part 2 - Building and Nesting Lists

| Question | Answer |
| :--- | :--- |
| What is `[a] + lst` when `a = 'peanut'` and `lst = ['butter', 'jelly']`? | `['peanut', 'butter', 'jelly']`. A one-item list is placed before the existing list. |
| What is `[s] + lst` when `s = ['banana', 'bread']` and `lst = ['butter', 'jelly']`? | `[['banana', 'bread'], 'butter', 'jelly']`. The entire list `s` becomes one element of the new list. |
| What is `[a] + lst` when `a = 'peanut'` and `lst = []`? | `['peanut']`. |
| What must the second value be in an expression such as `[a] + lst`? | It must be a list. List concatenation joins one list to another list. |
| Is a list allowed to contain different types of values? | Yes. A list may contain integers, strings, lists, or any mixture of objects. |
| What is the value of `lst[0][1]` when `lst = [['red', 'blue'], ['green', 'yellow']]`? | `'blue'`. The first index selects the first inner list; the second selects its second element. |

### Fundamental Questions About Prepending

| Question | Answer |
| :--- | :--- |
| What does `[x] + lst` do? | It creates a new list by placing `x` at the front of `lst`. |
| Does `[x] + lst` mutate `lst`? | No. It creates a new list. |
| How would you prepend a list as one element rather than add its contents individually? | Wrap the list in another pair of brackets, as in `[s] + lst`. |

---

## Part 3 - Empty Lists and Mutation

| Question | Answer |
| :--- | :--- |
| Is `len(lst) == 0` true or false when `lst = []`? | **True.** The list contains zero elements. |
| Is `len(lst) == 0` true or false when `lst = ['peanut', 'butter']`? | **False.** The list is not empty. |
| Is `not lst` true or false when `lst = []`? | **True.** An empty list is considered false in a Boolean context. |
| Is `not lst` true or false when `lst = ['peanut']`? | **False.** A non-empty list is considered true in a Boolean context. |
| What is `lst` after `lst = ['bread', 'butter']` followed by `lst[0] = 'toast'`? | `['toast', 'butter']`. Index assignment changes one value inside the existing list. |
| What happens after `s = 'bread'` followed by `s[0] = 't'`? | Python raises a `TypeError`. Strings are immutable, so individual characters cannot be replaced. |

### Fundamental Questions About Mutation

| Question | Answer |
| :--- | :--- |
| What does `lst[i] = value` do? | It replaces the value at index `i` inside an existing list. |
| Does `lst[i] = value` create a new list? | No. It changes the existing list in place. |
| Are lists mutable? | Yes. Their contents can be changed after the list is created. |
| Are strings mutable? | No. A string's individual characters cannot be changed in place. |

> ### Law of Mutation
> `lst[i] = value` changes an existing list in place; it never creates a new list, and it never works on a tuple or a string.

---

## Part 4 - Tuples

| Question | Answer |
| :--- | :--- |
| Is `(3, 4)` a list? | **No.** It is enclosed in parentheses, so it is a tuple. |
| Is `(3, 4)` a sequence? | **Yes.** Like a list, it contains ordered elements. |
| What is `pt[0]` when `pt = (3, 4)`? | `3`. |
| What is `pt[1]` when `pt = (3, 4)`? | `4`. |
| What happens if `pt[0] = 5` when `pt = (3, 4)`? | Python raises a `TypeError`. Tuples are immutable. |
| What happens if `pt = 5` after `pt = (3, 4)`? | The name `pt` now refers to the integer `5`. Reassigning a name is different from changing an element inside the tuple. |
| What is the result of `x, y = (3, 4)`? | `x` becomes `3` and `y` becomes `4`. This is tuple unpacking. |
| What is a useful representation for a fixed coordinate such as `x = 3`, `y = 4`? | The tuple `(3, 4)`. It is ordered and cannot be changed in place. |

### Fundamental Questions About the Tuple

| Question | Answer |
| :--- | :--- |
| How is a tuple like a list? | It is an ordered sequence and can be indexed and sliced. |
| How is a tuple different from a list? | A tuple is immutable. Its elements cannot be replaced after creation. |
| Can a tuple contain duplicate values? | Yes. Immutability does not imply uniqueness. |

> ### Law of the Tuple
> A tuple is an ordered, immutable sequence — reach for one whenever a value has a fixed shape that should never change after it is built, such as a coordinate.

---

## Part 5 - Sets

| Question | Answer |
| :--- | :--- |
| Is `{10, 11, 12}` a list? | **No.** It is a set. Sets are written with curly braces and contain values rather than key-value pairs. |
| What happens when you create `{10, 10, 11}`? | The duplicate `10` is removed. The set becomes `{10, 11}`. |
| Is `{}` an empty set? | **No.** `{}` is an empty dictionary. |
| How do you create an empty set? | `set()`. |
| Is `visited = {(0, 0), (0, 1)}` a dictionary? | **No.** It is a set containing two coordinate tuples. There are no colons separating keys and values. |
| Is `(0, 1) in visited` true or false when `visited = {(0, 0), (0, 1)}`? | **True.** The coordinate tuple is an element of the set. |
| What happens when `(0, 1)` is added to `visited` again? | Nothing changes. Sets contain each value at most once. |
| Can a set be indexed with `visited[0]`? | No. Sets do not provide positional indexing. |

### Fundamental Questions About Uniqueness

| Question | Answer |
| :--- | :--- |
| What does a set guarantee? | It guarantees that each element appears at most once. |
| Does a set preserve a useful sequence order? | No. Use a list or tuple when position and order matter. |
| When is a set useful? | When membership and uniqueness matter more than position. |

> ### Law of Uniqueness
> A set stores each value once and answers only one question well: does this belong? It does not promise an order.

---

## Part 6 - Dictionaries

| Question | Answer |
| :--- | :--- |
| Is `{'hp': 100, 'coins': 50}` a list or tuple? | **Neither.** It is a dictionary containing key-value pairs. |
| What is `player['hp']` when `player = {'hp': 100, 'coins': 50}`? | `100`. The key `'hp'` retrieves its associated value. |
| What is `player['score']` when `player = {'hp': 100, 'coins': 50}`? | No value. Python raises a `KeyError` because `'score'` is not a key. |
| What is `player.get('score', 0)` with the same dictionary? | `0`. `.get()` returns the default value when the key is missing. |
| What is `grid[(0, 1)]` when `grid = {(0, 0): 'wall', (0, 1): 'passage'}`? | `'passage'`. The tuple `(0, 1)` is being used as a dictionary key. |
| Can a list be used as a dictionary key? | No. Lists are mutable and therefore unhashable. |
| Can a tuple be used as a dictionary key? | Yes, provided its contents are hashable. Coordinate tuples are common dictionary keys. |
| Is `(0, 1) in grid` true when `(0, 1)` is a key in `grid`? | **True.** Membership in a dictionary checks its keys. |
| Is `'passage' in grid` true when `'passage'` is only a value in `grid`? | **False.** Use `'passage' in grid.values()` to search the values. |

### Fundamental Questions About Key Lookup

| Question | Answer |
| :--- | :--- |
| What does a dictionary associate? | A key with a value. |
| What must be unique in a dictionary? | The keys. Two entries cannot have the same key at the same time. |
| Can dictionary values repeat? | Yes. Several keys may refer to the same value. |
| What happens when a new key is assigned?<br><br>`grid = {(0, 0): 'wall'}`<br>`grid[(0, 1)] = 'passage'` | The dictionary grows to include the new key-value pair. |
| What happens when an existing key is assigned?<br><br>`grid[(0, 0)] = 'passage'` | The old value is replaced. The dictionary does not gain a second `(0, 0)` entry. |
| What is the value of `grid[(0, 0)]` afterward? | `'passage'`. |

---

## Part 7 - Comparing the Containers

| Question | Answer |
| :--- | :--- |
| Can a list change size? | Yes. Lists can gain or lose elements. |
| Can a tuple change size? | No. A tuple has a fixed length after creation. |
| Can a set change size? | Yes. Values can be added or removed. |
| Can a dictionary change size? | Yes. Key-value pairs can be added or removed. |
| Do lists require unique values? | No. Lists may contain duplicates. |
| Do tuples require unique values? | No. Tuples may contain duplicates. |
| Do sets require unique values? | Yes. A set stores each element at most once. |
| Do dictionaries require unique values? | No. Dictionary values may repeat. The keys must be unique. |
| Can lists and tuples be accessed by index? | Yes. |
| Can sets be accessed by index? | No. |
| Can dictionaries be accessed by numeric index? | Not in the list sense. Dictionaries are accessed by key. |
| Which structure would you use for an ordered collection that may contain duplicates? | A list. |
| Which structure would you use for a fixed coordinate such as `(x, y)`? | A tuple. |
| Which structure would you use for a collection of unique visited locations? | A set. |
| Which structure would you use for player statistics such as health and coins? | A dictionary. |

---

## Chapter Summary

| Container | Main idea |
| :--- | :--- |
| List | An ordered, mutable collection that allows duplicates. |
| Tuple | An ordered, immutable collection that allows duplicates. |
| Set | A mutable collection of unique values without positional indexing. |
| Dictionary | A mutable collection of unique keys mapped to values. |

The important question is not which container is “best.” The important question is:

> What relationship do I need to represent?

---

**==> You are not only storing values. You are choosing the shape of the world those values describe. <==**
