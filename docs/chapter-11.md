# Chapter 11: Persistence

In Chapter 9, Python learned how to represent a world. In Chapter 10, sets gave that world a way to remember groups of locations: which cells are blocked, which have been visited, which belong to a region.

All of that lives only in memory.

The grid, the blocked set, the visited set — every one of them can have rows, columns, coordinates, states, and neighbors that took real effort to generate.

But there is a problem.

When the program stops, the variables disappear.

So now we ask:

> How can my generated world survive the program?

The answer is persistence.

Persistence means storing data somewhere outside the running program so that the program can use it later.

We will begin with text files. Then we will use JSON to save the lists and dictionaries that describe our worlds.

The important shift is this:

```text
Variables live while the program runs.
Files can outlive the program.
```

---

## Part 11.1 - A String Is Data

Before a program can save data, it needs a form in which data can be written.

A string is a sequence of characters.

```python
message = 'hello, world'
```

| Question | Answer |
| :--- | :--- |
| True or false: `message` contains text. | **True.** Its value is the string `'hello, world'`. |
| What is the data type of `message`? | `str`. |
| Can a string contain numbers? | Yes, but those numbers are characters rather than integers. |
| What is the difference between `12` and `'12'`? | `12` is an integer; `'12'` is a string containing two characters. |
| What does `str(12)` produce? | The string `'12'`. |
| What does `int('12')` produce? | The integer `12`. |
| Why does this distinction matter when saving? | A file contains characters, so a program must know how to turn those characters back into useful data. |

### Fundamental Questions About Text

| Question | Answer |
| :--- | :--- |
| What does a string represent? | A sequence of characters. |
| Is every string automatically a Python list? | No. A string is its own data type, even though it can be indexed and looped over. |
| Can a string represent a sentence? | Yes. |
| Can a string represent a grid? | Yes, but the program must agree on how the characters represent rows, columns, and states. |
| Is text always enough for structured data? | No. Text is a starting point; formats such as JSON preserve more structure. |

---

## Part 11.2 - Writing a File

The `open` function gives a program access to a file.

```python
file = open('message.txt', 'w')
file.write('hello, world')
file.close()
```

| Question | Answer |
| :--- | :--- |
| True or false: this code creates or opens a file named `message.txt`. | **True.** The filename identifies the file. |
| What does the `'w'` mean? | Write mode. |
| What does `file.write(...)` do? | It writes characters into the file. |
| What does `file.close()` do? | It closes the file after the program is finished using it. |
| What happens if `message.txt` already exists? | Write mode replaces its previous contents. |
| Why can that be dangerous? | Old data can be erased if the program writes to the same file carelessly. |

### Fundamental Questions About Writing

| Question | Answer |
| :--- | :--- |
| Does `file.write(12)` write an integer as structured integer data? | No. File writing requires text; use `str(12)` when writing plain text. |
| Does `write` automatically add a newline? | No. Add `'\\n'` when a new line is wanted. |
| What does this write?<br><br>`file.write('one\\ntwo')` | The word `one`, then a newline, then the word `two`. |
| What does write mode do to the old file contents? | It starts a new file contents sequence, replacing what was there. |
| What should happen after writing is complete? | The file should be closed. |

---

## Part 11.3 - The `with` Statement

Python can close a file for us when a block is finished.

```python
with open('message.txt', 'w') as file:
    file.write('hello, world')
```

| Question | Answer |
| :--- | :--- |
| True or false: the file is available inside the indented block. | **True.** The name `file` refers to the opened file there. |
| What does the indentation identify? | The statements that use the open file. |
| What happens when the block ends? | Python closes the file. |
| Why is `with` preferred? | It keeps opening, using, and closing the file together. |
| Does `with` change what write mode means? | No. `'w'` still means that previous contents are replaced. |
| What is the general shape of the pattern? | Open a resource, use it inside a block, and let Python clean it up afterward. |

### Fundamental Questions About Managed Files

| Question | Answer |
| :--- | :--- |
| What does `with open(...) as file:` provide? | A temporarily available file object named `file`. |
| Where should file operations usually appear? | Inside the indented block. |
| What happens if an error occurs inside the block? | Python still handles the closing of the file as the block is left. |
| Is the file variable meant to be used after the block? | No. The file has been closed. |

---

## Part 11.4 - Reading a File

Writing stores characters. Reading retrieves them.

```python
with open('message.txt', 'r') as file:
    message = file.read()
```

| Question | Answer |
| :--- | :--- |
| What does the `'r'` mean? | Read mode. |
| What does `file.read()` return? | The file's contents as one string. |
| What is the type of `message`? | `str`. |
| If the file contains `hello, world`, what is `message`? | `'hello, world'`. |
| What happens if the file does not exist? | Python raises `FileNotFoundError`. |
| Does reading change the file? | No. Reading retrieves its contents without changing them. |

### Fundamental Questions About Reading

| Question | Answer |
| :--- | :--- |
| What does writing move? | Data from the program into a file. |
| What does reading move? | Data from a file into the program. |
| Is the result of `read()` automatically a list or dictionary? | No. It is a string until the program interprets it. |
| What must happen before a string can become a number? | The program must convert it, such as with `int(...)` or `float(...)`. |
| What must happen before a string can become a structured world? | The string must follow a format the program knows how to interpret. |

---

## Part 11.5 - Lines of Text

Files often contain one piece of information per line.

```python
with open('items.txt', 'w') as file:
    file.write('coin\\n')
    file.write('key\\n')
    file.write('potion\\n')
```

| Question | Answer |
| :--- | :--- |
| True or false: this file contains three lines. | **True.** Each `\\n` begins a new line. |
| What does `file.read()` return when this file is read? | One string containing the three lines and their newline characters. |
| What does `text.splitlines()` produce? | A list containing `'coin'`, `'key'`, and `'potion'`. |
| Does `splitlines()` keep the newline characters? | No. It separates the lines and removes the line endings. |
| What does this expression do?<br><br>`[int(line) for line in text.splitlines()]` | It converts each line of numeric text into an integer. |
| Is a line-based format useful for every data structure? | No. It works well for simple sequences, but nested structures need a richer format. |

### Fundamental Questions About Line-Based Data

| Question | Answer |
| :--- | :--- |
| What does `splitlines()` change? | One string into a list of strings. |
| What does `join` do?<br><br>`'\\n'.join(items)` | It combines strings into one string separated by newlines. |
| Are the values in a line-based file automatically typed? | No. They begin as strings. |
| Why can line-based storage become awkward? | The program must invent rules for nested lists, dictionaries, and different data types. |
| What kind of format will preserve more structure for us? | JSON. |

> ### Law of Line-Based Data
> A line-based text file is a list of strings waiting to be converted — it carries no types, no nesting, and no keys until the program supplies them.

---

## Part 11.6 - JSON Objects and Arrays

JSON is a text format for structured data.

Python lists and dictionaries have close JSON equivalents.

```python
import json

data = {
    'name': 'world',
    'width': 4,
    'height': 3,
    'cells': [0, 1, 0, 0, 2, 0, 1, 0, 0, 0, 1, 2]
}
```

| Question | Answer |
| :--- | :--- |
| True or false: `data` contains several related values under named keys. | **True.** It is a dictionary describing a world. |
| Which Python values appear in `data`? | A string, integers, and a list of integers. |
| What does JSON call a Python dictionary-like structure? | An object. |
| What does JSON call a Python list-like structure? | An array. |
| Can JSON represent strings and numbers? | Yes. |
| Can JSON represent nested lists and dictionaries? | Yes. |
| Can JSON represent arbitrary Python objects? | No. JSON supports a smaller collection of common data types. |

### Fundamental Questions About JSON Data

| Question | Answer |
| :--- | :--- |
| Which Python values naturally fit JSON? | `dict`, `list`, `str`, `int`, `float`, `bool`, and `None`. |
| What does Python `None` become in JSON? | `null`. |
| What does Python `True` become in JSON? | `true`. |
| What does Python `False` become in JSON? | `false`. |
| Why is JSON useful for persistence? | It stores structured data as readable text that many programs can understand. |
| Is JSON the same thing as a Python dictionary? | No. A dictionary is a Python object; JSON is a text representation. |

> ### Law of JSON Data
> JSON stores objects, arrays, strings, numbers, booleans, and null — not tuples, not sets, and not arbitrary Python objects; anything else must be converted before it is saved.

---

## Part 11.7 - Saving JSON with `dump`

The `json` module can write structured Python data into a file.

```python
import json

data = {
    'name': 'world',
    'width': 4,
    'height': 3,
    'cells': [0, 1, 0, 0, 2, 0, 1, 0, 0, 0, 1, 2]
}

with open('world.json', 'w') as file:
    json.dump(data, file, indent=2)
```

| Question | Answer |
| :--- | :--- |
| True or false: `json.dump` writes `data` into the file. | **True.** It converts the structured value into JSON text. |
| What does `indent=2` do? | It makes the JSON easier for people to read by adding indentation. |
| What does the file contain after this code? | A text representation of the dictionary, including its keys, values, and nested list. |
| Does `json.dump` change `data` into a string in the program? | No. It writes a representation to the file; `data` remains a dictionary. |
| What is the purpose of the `.json` ending? | It identifies the file as JSON by convention. |
| Could the file be opened in a text editor? | Yes. JSON is plain text. |

### Fundamental Questions About Serialization

| Question | Answer |
| :--- | :--- |
| What is serialization? | Converting an in-memory value into a form that can be stored or transmitted. |
| What does `json.dump` serialize? | A Python value into JSON text in a file. |
| Does serialization preserve every Python detail? | No. It preserves the data that the JSON format can represent. |
| What should a saved world include? | Enough information to reconstruct its dimensions, states, and important rules. |

---

## Part 11.8 - Loading JSON with `load`

The reverse operation is `json.load`.

```python
import json

with open('world.json', 'r') as file:
    data = json.load(file)
```

| Question | Answer |
| :--- | :--- |
| What does `json.load` do? | It reads JSON text and constructs the corresponding Python value. |
| What is the type of `data` here? | `dict`, because the saved JSON began as an object. |
| What is `data['width']`? | `4`. |
| What is `data['cells']`? | The list of twelve cell values. |
| Does loading run the original program? | No. It reconstructs data, not the history of the program that created it. |
| What is the pair of operations for persistence? | Save with `dump`; restore with `load`. |

### Fundamental Questions About Round Trips

| Question | Answer |
| :--- | :--- |
| What is a round trip? | Saving a value and then loading it again. |
| What should survive a successful round trip? | The meaningful data and its structure. |
| Is the loaded object necessarily the same object in memory? | No. It is a newly constructed Python value with the same represented data. |
| Why test a round trip? | A program may save something successfully but still fail to reconstruct it correctly. |

---

## Part 11.9 - Saving a Flat Gridworld

Chapter 9 gave us a flat `4 x 3` world.

```python
world = {
    'width': 4,
    'height': 3,
    'cells': [
        0, 1, 0, 0,
        2, 0, 1, 0,
        0, 0, 1, 2
    ]
}
```

| Question | Answer |
| :--- | :--- |
| True or false: this dictionary contains enough information to recover the rectangular shape. | **True.** It contains both `width` and `height`. |
| What does `cells` store? | The twelve cell states in flat order. |
| Why save `width` as well as `cells`? | Without the width, the row structure cannot be recovered from the flat list. |
| Why save `height` too? | It records the intended world boundary and allows bounds checks after loading. |

> ### Law of a Saved World
> Save enough to reconstruct the shape of the world, not just its appearance — a flat grid needs its width and height saved alongside its cells, or the row structure is gone for good.

| What does `world['cells'][5]` represent? | The state at coordinate `(1, 1)` when the width is `4`. |
| What should happen after loading this world? | The program can use the same coordinate-to-index rules from Chapter 9. |

```python
import json

with open('world.json', 'w') as file:
    json.dump(world, file, indent=2)
```

### Fundamental Questions About a Saved World

| Question | Answer |
| :--- | :--- |
| Does saving a grid require saving its printed appearance? | No. Save the data needed to reconstruct the grid. |
| Does the file need to save every coordinate separately? | No. A flat list plus dimensions is enough for a dense grid. |
| What is the advantage of saving data rather than a screenshot? | The program can continue to inspect, change, and reason about the world. |
| What is the difference between a saved world and a saved picture? | A saved world preserves structure and state; a picture preserves appearance. |

---

## Part 11.10 - Loading and Using the World

Loading is only useful if the reconstructed data can be used.

```python
import json

with open('world.json', 'r') as file:
    world = json.load(file)

width = world['width']
height = world['height']
grid = world['cells']
```

| Question | Answer |
| :--- | :--- |
| What does `world['width']` provide? | The number of columns in each row. |
| What does `world['height']` provide? | The number of rows in the world. |
| What does assigning `world['cells']` to `grid` do? | It gives the loaded flat list a name that matches its role in Chapter 9. |
| Can `grid[index_of(1, 1, width)]` now be used? | Yes. It accesses the restored state at `(1, 1)`. |
| Can the loaded grid be changed? | Yes. Loading creates ordinary mutable Python data. |
| Has changing the loaded grid changed the file? | No. The file must be written again to persist the new change. |

### Fundamental Questions About Load, Change, Save

| Question | Answer |
| :--- | :--- |
| What is the basic persistence cycle? | Load data, use or change data, then save data again. |
| Does changing a variable automatically update its file? | No. Memory and storage are separate. |
| When should a program save? | Whenever the current state needs to survive a future shutdown or restart. |
| What can go wrong if a program saves too rarely? | Recent changes may be lost. |
| What can go wrong if a program saves too often? | Saving may become slow or create unnecessary file operations. |

---

## Part 11.11 - Lists, Tuples, and JSON

JSON can represent a list, but it does not have a separate tuple type.

```python
data = {
    'start': (1, 1),
    'targets': [(0, 0), (3, 2)]
}
```

| Question | Answer |
| :--- | :--- |
| True or false: JSON has a special tuple type. | **False.** JSON has arrays, which are represented as Python lists after loading. |
| What happens to `'targets'` after a JSON round trip? | Its coordinate pairs load as lists, such as `[[0, 0], [3, 2]]`. |
| What happens to `'start'` after a JSON round trip? | The tuple `(1, 1)` loads as the list `[1, 1]`. |
| Is the coordinate information lost? | No. The two values are still present, but the Python container type has changed. |
| How could the program restore the tuple?<br><br>`start = tuple(data['start'])` | It converts the loaded list back into a tuple. |
| Why does this matter for gridworld code? | Tuples are convenient coordinate values, but JSON restores arrays as lists. |

### Fundamental Questions About Representation Boundaries

| Question | Answer |
| :--- | :--- |
| Does a saved format have to preserve the exact Python type of every container? | No. It must preserve enough meaning for the program to reconstruct what it needs. |
| What should happen when a tuple is loaded as a list? | Convert it back if later code depends on tuple behavior or tuple keys. |
| Is a list always a bad coordinate representation? | No. It can still hold the two coordinate values; it is simply mutable and unhashable. |
| What is a representation boundary? | A place where data moves between two systems with different rules, such as Python memory and JSON text. |

> ### Law of Representation Boundaries
> A tuple saved to JSON and loaded back becomes a list — convert it back explicitly wherever the program relies on tuple behavior, such as using it as a dictionary key.

---

## Part 11.12 - Saving Sparse Coordinates

Chapter 9 introduced sparse dictionaries, and Chapter 10 introduced sparse coordinate sets.

JSON object keys must be strings, so a dictionary with tuple keys cannot be saved directly.

Instead, save coordinates as data inside a list.

```python
blocked = {(1, 0), (2, 1), (3, 2)}

data = {
    'blocked': [list(point) for point in blocked]
}
```

| Question | Answer |
| :--- | :--- |
| True or false: `blocked` is a set of coordinate tuples. | **True.** Each member is a tuple such as `(1, 0)`. |
| What does `list(point)` do? | It converts one coordinate tuple into a two-item list. |
| What does the list comprehension produce? | A list of coordinate lists suitable for JSON. |
| Why not use `blocked` directly as a JSON dictionary key structure? | JSON object keys must be strings, not tuple values. |
| Does the saved list preserve that these coordinates were originally a set? | No. The program must reconstruct the set after loading. |

```python
with open('blocked.json', 'w') as file:
    json.dump(data, file, indent=2)
```

After loading:

```python
with open('blocked.json', 'r') as file:
    data = json.load(file)

blocked = {tuple(point) for point in data['blocked']}
```

| Question | Answer |
| :--- | :--- |
| What does `tuple(point)` do? | It converts each loaded coordinate list back into a tuple. |
| What does the surrounding `{...}` create? | A set comprehension. |
| What is the type of the restored `blocked`? | `set`. |
| What is the type of each restored member? | `tuple`. |
| What is the larger pattern? | Save data in a JSON-friendly form; reconstruct the richer Python representation after loading. |

### Fundamental Questions About Reconstruction

| Question | Answer |
| :--- | :--- |
| Does loading always finish the job? | No. Sometimes the program must reconstruct tuples, sets, or other intended structures. |
| What is a serialization format? | A compromise between what is convenient to store and what is useful to reconstruct. |
| Is converting a set to a list a loss of membership information? | Not necessarily. The list still contains the members, and the program can rebuild the set. |
| What must be documented when saving custom structures? | The shape of the saved data and the steps needed to restore it. |

---

## Part 11.13 - A Save and Load Function

The process can be given names.

```python
import json

def save_world(filename, world):
    with open(filename, 'w') as file:
        json.dump(world, file, indent=2)

def load_world(filename):
    with open(filename, 'r') as file:
        return json.load(file)
```

| Question | Answer |
| :--- | :--- |
| What does `save_world` receive? | A filename and a world to store. |
| What does `save_world` return? | Nothing explicitly, so it returns `None`. Its effect is writing the file. |
| What does `load_world` receive? | A filename. |
| What does `load_world` return? | The Python value reconstructed from the JSON file. |
| Why is naming these processes useful? | The rest of the program can ask to save or load without repeating file-handling details. |
| What idea from Chapter 6 is being reused? | A function gives a name to a process so that the process can be called again. |

### Fundamental Questions About Persistent Abstraction

| Question | Answer |
| :--- | :--- |
| Does `save_world` need to know how the world was generated? | No. It only needs a JSON-compatible representation. |
| Does `load_world` need to know the future rules of the game? | No. It reconstructs the saved data. |
| What should the rest of the program depend on? | The meaning of the loaded world, not the details of opening and closing files. |
| What has persistence added to our programs? | A world can now be generated, saved, stopped, loaded, and used again. |

---

## Part 11.14 - Missing Files and Defaults

A program may try to load a world before a save file exists.

```python
import json

def load_world(filename):
    try:
        with open(filename, 'r') as file:
            return json.load(file)
    except FileNotFoundError:
        return {
            'width': 4,
            'height': 3,
            'cells': [0] * 12
        }
```

| Question | Answer |
| :--- | :--- |
| True or false: the `try` block attempts the ordinary load. | **True.** Python first tries to open and read the file. |
| What happens if the file exists? | The loaded world is returned. |
| What happens if the file is missing? | Python raises `FileNotFoundError`, and the `except` block handles it. |
| What does the `except` block return? | A new empty `4 x 3` world. |
| Why is this useful? | A first run can begin with a default world instead of crashing. |
| Is every file error the same as a missing file? | No. A malformed JSON file is a different problem and may need different handling. |

### Fundamental Questions About Defaults

| Question | Answer |
| :--- | :--- |
| What is a default world? | A valid starting value used when no saved world is available. |
| Should a default have the same structure as a loaded world? | Yes. The rest of the program should not need two completely different representations. |
| What does this design protect? | The boundary between persistence and the rest of the game or simulation. |
| What should happen when a save file is corrupted? | The program should report or handle the problem deliberately rather than silently inventing a new world. |

---

## Part 11.15 - Predict the Result

Do not run these immediately. Predict the final value and the data type.

| Question | Answer |
| :--- | :--- |
| What is `result`?<br><br>`text = str(42)`<br>`result = text` | `'42'`, a string. |
| What is `result`?<br><br>`text = '42'`<br>`result = int(text)` | `42`, an integer. |
| What is `result`?<br><br>`data = {'width': 4, 'cells': [0, 1, 0]}`<br>`result = data['width']` | `4`, an integer. |
| What does `json.load` return for a saved dictionary? | A Python dictionary. |
| What is `result`?<br><br>`data = {'start': [1, 1]}`<br>`result = tuple(data['start'])` | `(1, 1)`, a tuple. |
| What is `result`?<br><br>`blocked = {tuple(point) for point in [[1, 0], [2, 1]]}`<br>`result = (2, 1) in blocked` | `True`, a Boolean. |
| What is `grid`?<br><br>`world = {'cells': [0, 1, 0, 0]}`<br>`grid = world['cells']` | `[0, 1, 0, 0]`, a list. |
| What does this function return when the file is missing?<br><br>`def load_value(filename):`<br>&emsp;`try:`<br>&emsp;&emsp;`with open(filename) as file:`<br>&emsp;&emsp;&emsp;`return file.read()`<br>&emsp;`except FileNotFoundError:`<br>&emsp;&emsp;`return ''` | An empty string. |
| What survives a save/load round trip?<br><br>`world = {'width': 4, 'height': 3, 'cells': [0] * 12}` | The dictionary's represented keys and values, reconstructed as new Python data. |

---

## Chapter Summary

| Question | Answer |
| :--- | :--- |
| What is persistence? | Storing data so that it can survive after a running program ends. |
| What does a text file store? | Characters. |
| What does `with open(...) as file` provide? | A managed file that is automatically closed after the block. |
| What does `file.write` do? | Writes text to a file. |
| What does `file.read` do? | Reads a file's contents as a string. |
| What does JSON provide? | A readable text format for structured data. |
| What does `json.dump` do? | Serializes Python data into a JSON file. |
| What does `json.load` do? | Reconstructs Python data from a JSON file. |
| What is a round trip? | Saving data and loading it again. |
| What is reconstruction? | Converting loaded JSON-friendly values back into the structures the program wants to use. |
| What is the central idea? | A generated world becomes persistent when the program can represent it, save it, load it, and reconstruct the rules needed to use it again. |

---

**==> A variable remembers while the program runs. A file remembers after it stops. <==**
