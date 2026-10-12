# Chapter 9: Two-Dimensional Worlds in One Dimension

In Chapter 8, Python learned how to generate variation.

Now we need somewhere for that variation to live.

> How can a program represent a world that has rows, columns, locations, and states?

We will begin with the familiar picture of a two-dimensional list. Then we will move quickly toward the representation we will use more often: a flat one-dimensional list with a coordinate lookup.

The important shift is this:

```text
The world looks two-dimensional.
The data does not have to be.
```

---

## Part 9.1 - A Two-Dimensional List

A nested list is the most direct way to picture a grid.

```python
grid = [
    [0, 0, 0],
    [0, 1, 0],
    [0, 0, 0]
]
```

| Question | Answer |
| :--- | :--- |
| True or false: `grid` contains three rows. | **True.** The outer list contains three inner lists. |
| How many values are in each row? | Three. |
| Why are `0` and `1` useful cell values here? | They are easy to print and make the rectangular structure visible. |
| What is `grid[0]`? | `[0, 0, 0]`, the first row. |
| What is `grid[1]`? | `[0, 1, 0]`, the second row. |
| What is `grid[1][1]`? | `1`. The first index selects the row; the second selects the column. |
| What is `grid[2][0]`? | `0`. |
| What does `grid[y][x]` mean? | Look up row `y`, then column `x`. |
| What is the coordinate of the `1` if coordinates are written `(x, y)`? | `(1, 1)`. |

### Fundamental Questions About the Nested Grid

| Question | Answer |
| :--- | :--- |
| What does the first index in `grid[y][x]` select? | A row. |
| What does the second index select? | A column inside that row. |
| Why is `grid[y][x]` not the same as `grid[x][y]`? | The two expressions reverse the meanings of the coordinates. |
| What must be true for the grid to be rectangular? | Every row must contain the same number of values. |
| Is a nested list a useful mental model for a grid? | Yes. It makes the rows and columns immediately visible. |
| Will we rely on nested lists for every operation? | No. We will use this picture to understand a flatter representation. |

---

## Part 9.2 - Printing a Grid

Small numeric grids can be inspected directly.

| Question | Answer |
| :--- | :--- |
| What does this loop print?<br><br>`for row in grid:`<br>&emsp;`print(row)` | One row per line, such as `[0, 0, 0]`, then `[0, 1, 0]`, then `[0, 0, 0]`. |
| What does `print(*row)` do? | It prints the values in the row separated by spaces, such as `0 0 0`. |
| What does this nested loop visit?<br><br>`for row in grid:`<br>&emsp;`for cell in row:`<br>&emsp;&emsp;`print(cell)` | Every cell, row by row. |
| Why are numeric cell states useful for early grid work? | The printed output remains compact, aligned, and easy to inspect. |
| What does changing `grid[1][1]` from `1` to `2` represent? | A change in the state of one cell without changing the shape of the grid. |

---

## Part 9.3 - The Flat Grid

The same `4 x 3` world can be represented as one list:

```python
grid = [
    0, 0, 0, 0,
    0, 1, 0, 0,
    0, 0, 0, 0
]
```

| Question | Answer |
| :--- | :--- |
| True or false: the flat grid still contains twelve cells. | **True.** The nested shape disappeared, but the number of values did not. |
| What is the width of this grid? | `4`. |
| What is the height? | `3`. |
| What is `grid[0]`? | The first cell, not the first row. |
| What is `grid[5]`? | `1`, the cell at coordinate `(1, 1)`. |
| What is the flat index of the upper-left coordinate `(0, 0)`? | `0`. |
| What is the flat index of the coordinate `(1, 1)`? | `5`. |
| What is the flat index of the lower-right coordinate `(3, 2)`? | `11`. |
| Does the flat list visibly show its rows when printed? | No. The program must know the width in order to recover the row structure. |

### Fundamental Questions About the Flat World

| Question | Answer |
| :--- | :--- |
| What does the flat list preserve? | The cells and their order. |
| What does the flat list hide? | The row boundaries. |
| What additional information must accompany a flat grid? | At least its width, and usually its height as well. |
| Why use a flat representation? | Coordinate conversion, iteration, storage, and interaction with image-like data become explicit and uniform. |

> ### Law of the Flat World
> A flat list preserves every cell and its order but hides the row boundaries — the width must travel with the list wherever it goes.

---

## Part 9.4 - The Coordinate-to-Index Lookup

For a flat grid, we need a rule that converts `(x, y)` into one list index.

For a grid with width `4` and height `3`:

```python
lookup = {
    (0, 0): 0, (1, 0): 1, (2, 0): 2, (3, 0): 3,
    (0, 1): 4, (1, 1): 5, (2, 1): 6, (3, 1): 7,
    (0, 2): 8, (1, 2): 9, (2, 2): 10, (3, 2): 11
}
```

| Question | Answer |
| :--- | :--- |
| True or false: `lookup[(1, 1)]` is `5`. | **True.** The coordinate `(1, 1)` maps to flat index `5`. |
| What is `lookup[(2, 0)]`? | `2`. |
| What is `lookup[(0, 1)]`? | `4`. |
| What is `lookup[(3, 2)]`? | `11`. |
| Why does `(0, 1)` map to `4`? | One complete row of width `4` comes before row `1`. |
| Why does `(2, 1)` map to `6`? | Skip four values for row `0`, then move two columns into row `1`. |
| What pattern appears in the lookup? | Every row begins `width` positions after the previous row. |

### Fundamental Questions About the Gridworld Lookup

| Question | Answer |
| :--- | :--- |
| What does the lookup dictionary represent? | The relationship between a spatial coordinate and its storage index. |
| Why build the lookup explicitly first? | It makes the relationship visible before replacing it with a formula. |
| What is the formula suggested by the lookup? | `index = y * width + x`. |
| What does `y * width` do? | It skips all complete rows above the target row. |
| What does `+ x` do? | It moves to the target column within that row. |

> ### Law of the Gridworld Lookup
> The coordinate-to-index formula is `index = y * width + x`: `y * width` skips complete rows, and `+ x` moves across the current one.

---

## Part 9.5 - The Coordinate-to-Index Function

The explicit lookup can be generalized into a function.

```python
def index_of(x, y, width):
    return y * width + x
```

| Question | Answer |
| :--- | :--- |
| What is `index_of(0, 0, 4)`? | `0`. |
| What is `index_of(1, 0, 4)`? | `1`. |
| What is `index_of(0, 1, 4)`? | `4`. |
| What is `index_of(2, 1, 4)`? | `6`. |
| What is `index_of(1, 2, 4)`? | `9`. Two complete rows of width `4` contribute `8`, then column `1` adds `1`. |
| What is `index_of(3, 2, 4)`? | `11`. |
| Does `index_of` need the grid itself? | No. It needs the coordinate and the width. |
| Why is that useful? | The coordinate rule becomes independent of the particular cell values stored in the grid. |

### Fundamental Questions About Mapping

| Question | Answer |
| :--- | :--- |
| What is a mapping? | A rule that translates one representation into another. |
| What does `index_of` map? | A two-dimensional coordinate to a one-dimensional index. |
| Is `(x, y)` itself stored in the flat list? | No. The coordinate is converted into an index before the list is accessed. |
| What is the flat-grid access pattern? | `grid[index_of(x, y, width)]`. |

---

## Part 9.6 - Reading and Changing Flat Cells

Once the mapping exists, a flat grid can still be treated as a world.

| Question | Answer |
| :--- | :--- |
| What is the value at `(1, 1)`?<br><br>`grid = [0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0]`<br>`width = 4` | `grid[index_of(1, 1, 4)]`, which is `1`. |
| What happens after `grid[index_of(0, 0, 4)] = 2`? | The first cell becomes `2`. |
| What happens after `grid[index_of(3, 2, 4)] = 2`? | The last cell becomes `2`. |
| Does changing a flat cell change the grid's width? | No. It changes only one stored state. |
| What does `grid[index_of(x, y, width)]` represent? | The state at world coordinate `(x, y)`. |
| What is the important difference from `grid[y][x]`? | The flat form performs an explicit coordinate-to-index conversion first. |

### Fundamental Questions About Flat State

| Question | Answer |
| :--- | :--- |
| What does a cell value represent? | The current state at one coordinate. |
| What does the index represent? | Where that state is stored in the one-dimensional list. |
| What does the coordinate represent? | Where the state exists in the two-dimensional world. |
| Can one list value be understood spatially? | Yes, once the width and mapping rule are known. |

---

## Part 9.7 - Bounds for a Flat Grid

Bounds now become part of the coordinate-to-index process.

| Question | Answer |
| :--- | :--- |
| True or false: `(3, 2)` is valid for a grid with width `4` and height `3`. | **True.** `0 <= x < 4` and `0 <= y < 3`. |
| Is `(4, 2)` valid? | No. `x = 4` is outside the valid columns `0, 1, 2, 3`. |
| Is `(3, 3)` valid? | No. `y = 3` is outside the valid rows `0, 1, 2`. |
| Is `(-1, 0)` valid in the intended world coordinate system? | No. Negative coordinates are outside this grid. |
| What is the largest valid flat index for width `4` and height `3`? | `11`, because there are `4 * 3 = 12` cells. |
| What is the difference between an invalid coordinate and an invalid flat index? | A coordinate can be outside the rectangular world even before it is converted into an index. A flat index can be outside the list's storage range. |

```python
def in_bounds(x, y, width, height):
    return 0 <= x < width and 0 <= y < height
```

| Question | Answer |
| :--- | :--- |
| What is `in_bounds(0, 0, 4, 3)`? | `True`. |
| What is `in_bounds(3, 2, 4, 3)`? | `True`. |
| What is `in_bounds(4, 1, 4, 3)`? | `False`. |
| What is `in_bounds(3, 3, 4, 3)`? | `False`. |
| Why should the coordinate be checked before calling `index_of`? | The coordinate is a world-level question. Bounds should be resolved before storage is accessed. |

### Fundamental Questions About Coordinate Safety

| Question | Answer |
| :--- | :--- |
| What must be true before accessing a flat cell? | The coordinate must be in bounds. |
| What is a safe access pattern?<br><br>`if in_bounds(x, y, width, height):`<br>&emsp;`value = grid[index_of(x, y, width)]` | Check the coordinate, map it to an index, then access the list. |
| Why not rely on Python's negative indexing? | A negative index may be valid to Python while still being invalid in the intended world. |

---

## Part 9.8 - Mapping an Index Back to Coordinates

The mapping can work in both directions.

```python
def point_of(index, width):
    x = index % width
    y = index // width
    return (x, y)
```

| Question | Answer |
| :--- | :--- |
| What is `point_of(0, 4)`? | `(0, 0)`. |
| What is `point_of(3, 4)`? | `(3, 0)`. |
| What is `point_of(4, 4)`? | `(0, 1)`. |
| What is `point_of(7, 4)`? | `(3, 1)`. |
| What does `index % width` calculate? | The column `x`. |
| What does `index // width` calculate? | The row `y`. |
| Why must `width` be known? | The same index maps to different coordinates for different row widths. |

### Fundamental Questions About Reversible Mapping

| Question | Answer |
| :--- | :--- |
| What does `index_of` do? | Coordinate to index. |
| What does `point_of` do? | Index to coordinate. |
| What should happen when the functions are used together?<br><br>`point_of(index_of(x, y, width), width)` | The original `(x, y)` should be recovered, assuming the coordinate was valid. |
| Why is reversibility useful? | A program can move between spatial reasoning and storage reasoning without losing location information. |

> ### Law of Reversible Mapping
> `index_of` and `point_of` are inverses: converting a coordinate to an index and back should always recover the original coordinate.

---

## Part 9.9 - An Explicit Neighbor Dictionary

Before generating neighbors with a formula, write them down.

For a `4 x 3` world, the valid four-direction neighbors are:

```python
neighbors = {
    (0, 0): [(1, 0), (0, 1)],
    (1, 0): [(2, 0), (0, 0), (1, 1)],
    (2, 0): [(3, 0), (1, 0), (2, 1)],
    (3, 0): [(2, 0), (3, 1)],
    (0, 1): [(1, 1), (0, 2), (0, 0)],
    (1, 1): [(2, 1), (0, 1), (1, 2), (1, 0)],
    (2, 1): [(3, 1), (1, 1), (2, 2), (2, 0)],
    (3, 1): [(2, 1), (3, 2), (3, 0)],
    (0, 2): [(1, 2), (0, 1)],
    (1, 2): [(2, 2), (0, 2), (1, 1)],
    (2, 2): [(3, 2), (1, 2), (2, 1)],
    (3, 2): [(2, 2), (3, 1)]
}
```

| Question | Answer |
| :--- | :--- |
| True or false: the corner `(0, 0)` has four valid neighbors. | **False.** It has only `(1, 0)` and `(0, 1)`. |
| What are the neighbors of `(0, 0)`? | `[(1, 0), (0, 1)]`. |
| What are the neighbors of `(1, 0)`? | `[(2, 0), (0, 0), (1, 1)]`. |
| Why does `(1, 0)` have three neighbors rather than four? | It is on the top edge, so the coordinate above it is outside the world. |
| What are the neighbors of the interior cell `(1, 1)`? | `[(2, 1), (0, 1), (1, 2), (1, 0)]`. |
| Why does `(1, 1)` have four neighbors in this `4 x 3` world? | It is away from every edge, so all four one-step directions remain in bounds. |
| What does `neighbors[(3, 2)]` return? | `[(2, 2), (3, 1)]`. |

### Fundamental Questions About Explicit Adjacency

| Question | Answer |
| :--- | :--- |
| What does the neighbor dictionary represent? | A direct relationship between each coordinate and the coordinates reachable in one step. |
| Why begin with an explicit dictionary? | It makes adjacency concrete before we ask Python to generate it. |
| Does every coordinate have the same number of neighbors? | No. Corners, edges, and interior locations differ. |
| What larger idea is visible here? | A world can be represented as relationships between locations, not only as stored cell values. |

> ### Law of Explicit Adjacency
> Write a small neighbor table out by hand before writing the rule that generates it — corners and edges have fewer neighbors than interior cells, and the exceptions are easy to miss if you only ever look at the formula.

---

## Part 9.10 - Generating Neighbors from Directions

The explicit dictionary suggests a reusable rule.

```python
directions = [
    (1, 0),
    (-1, 0),
    (0, 1),
    (0, -1)
]
```

| Question | Answer |
| :--- | :--- |
| What does `(1, 0)` represent? | One step right. |
| What does `(-1, 0)` represent? | One step left. |
| What does `(0, 1)` represent? | One step down if `y` increases downward. |
| What does `(0, -1)` represent? | One step up if `y` increases downward. |
| What does this expression generate?<br><br>`[(x + dx, y + dy) for dx, dy in directions]` | Four candidate neighboring coordinates. |
| Are all four candidates necessarily valid? | No. Candidates near an edge may leave the world. |
| How do we produce only valid neighbors? | Add an `if in_bounds(...)` condition to the comprehension. |

```python
def neighbors4(x, y, width, height):
    return [
        (x + dx, y + dy)
        for dx, dy in directions
        if in_bounds(x + dx, y + dy, width, height)
    ]
```

| Question | Answer |
| :--- | :--- |
| What is `neighbors4(0, 0, 4, 3)`? | `[(1, 0), (0, 1)]`. |
| What is `neighbors4(1, 0, 4, 3)`? | `[(2, 0), (0, 0), (1, 1)]`, assuming the directions are listed right, left, down, up. |
| What is `neighbors4(1, 1, 4, 3)`? | `[(2, 1), (0, 1), (1, 2), (1, 0)]`. |
| What relationship exists between the explicit dictionary and this function? | The dictionary records the result for one small world. The function describes the rule that can generate the result for any world size. |

### Fundamental Questions About Generated Adjacency

| Question | Answer |
| :--- | :--- |
| What is a candidate neighbor? | A coordinate produced by applying one direction offset. |
| What is a valid neighbor? | A candidate that passes the bounds test. |
| Why separate candidate generation from validation? | The direction rule and the world boundary rule are different ideas. |
| What does the final neighbor list represent? | The locations reachable in one local step. |

---

## Part 9.11 - Flat Grid State and Neighbor Access

The neighbor coordinates can now be used to read a flat grid.

```python
grid = [
    0, 1, 0, 0,
    2, 0, 1, 0,
    0, 0, 1, 2
]

width = 4
height = 3
point = (1, 0)
```

| Question | Answer |
| :--- | :--- |
| What is the state at `point`? | `grid[index_of(1, 0, 4)]`, which is `1`. |
| What are the valid neighbors of `point`? | `(2, 0)` and `(0, 0)`, plus `(1, 1)` if the direction order includes down. |
| What are the states of those neighbors? | At `(2, 0)`, the state is `0`; at `(0, 0)`, it is `0`; at `(1, 1)`, it is `0`. |
| How can we inspect neighboring states? | Generate valid neighbor coordinates, convert each to an index, and read the flat grid. |
| Why is the coordinate-to-index function important here? | It allows spatial relationships to operate on one-dimensional storage. |

### Fundamental Questions About Two Levels

| Question | Answer |
| :--- | :--- |
| What level is a neighbor function operating at? | The spatial level: coordinates and relationships. |
| What level is `index_of` operating at? | The storage level: coordinates and list positions. |
| What level is `grid[index]` operating at? | The state level: the value stored at a location. |
| Why keep these levels distinct? | It lets the same spatial logic work with different storage choices. |

---

## Part 9.12 - Sparse Spatial Dictionaries

A dictionary can represent only the locations that need explicit states.

```python
world = {
    (1, 0): 1,
    (2, 1): 2
}
```

| Question | Answer |
| :--- | :--- |
| True or false: every coordinate in the world must be stored in `world`. | **False.** Only two coordinates are explicitly present. |
| What is `world[(1, 0)]`? | `1`. |
| What is `world[(2, 1)]`? | `2`. |
| What happens with `world[(0, 0)]`? | A direct lookup raises a `KeyError` because the key is missing. |
| What is `world.get((0, 0), 0)`? | `0`. The missing coordinate is treated as the default state. |
| What does a missing key represent in this design? | An implicit ordinary state such as empty space. |
| Why might a sparse dictionary be useful? | It stores only unusual, occupied, or important locations. |

### Fundamental Questions About Dense and Sparse Worlds

| Question | Answer |
| :--- | :--- |
| What is a dense representation? | A structure that stores every cell, including cells with ordinary states. |
| What is a sparse representation? | A structure that stores only selected cells and interprets missing cells through a default. |
| Which representation is naturally easy to print as a rectangle? | A dense grid, especially a nested or flat grid with known width. |
| Which representation is naturally useful for a mostly empty world? | A sparse dictionary. |
| Can both representations use coordinate tuples? | Yes. Coordinates are independent of the storage choice. |

> ### Law of Dense and Sparse Worlds
> A dense grid stores every cell; a sparse dictionary stores only the cells worth naming and treats everything else as a default — choose based on how much of the world is “ordinary.”

---

## Part 9.13 - State Transitions

A representation becomes a world when rules can change its state.

| Question | Answer |
| :--- | :--- |
| What is the current state of `(1, 0)`?<br><br>`grid = [0, 1, 0, 0, 2, 0, 1, 0, 0, 0, 1, 2]`<br>`width = 4` | `1`. |
| What happens after `grid[index_of(1, 0, width)] = 2`? | The cell at `(1, 0)` changes from `1` to `2`. |
| What should happen before moving from `(1, 0)` to `(2, 0)`? | Check that `(2, 0)` is in bounds and decide whether its state permits movement. |
| What does a movement rule need? | A current coordinate, an offset, the world dimensions, and a rule for acceptable destination states. |
| What is a state transition? | A rule that changes the world from one valid arrangement of values to another. |
| Why is this not yet pathfinding? | It changes or inspects one local relationship. Pathfinding must compare many possible routes. |

### Fundamental Questions About Stateful Space

| Question | Answer |
| :--- | :--- |
| What does a coordinate identify? | A location. |
| What does a cell value describe? | The current state at that location. |
| What does a neighbor relationship describe? | Which locations can be considered nearby or reachable in one local step. |
| What does a transition rule describe? | How one state becomes another. |
| Why separate these ideas? | A clear separation makes later procedural algorithms easier to build and debug. |

---

## Part 9.14 - Predict the Result

Do not run these immediately. Predict the final value and the data type.

| Question | Answer |
| :--- | :--- |
| What is `grid[1][1]`?<br><br>`grid = [[0, 0], [0, 1]]` | `1`, an integer. |
| What is `index`?<br><br>`width = 4`<br>`x, y = (2, 1)`<br>`index = y * width + x` | `6`, an integer. |
| What is `point`?<br><br>`width = 4`<br>`index = 9`<br>`point = (index % width, index // width)` | `(1, 2)`, a tuple. |
| What is `grid`?<br><br>`grid = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]`<br>`width = 4`<br>`grid[index_of(3, 2, width)] = 1` | `[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1]`, a list. |
| What is `result`?<br><br>`world = {(2, 1): 2}`<br>`result = world.get((0, 0), 0)` | `0`, an integer. |
| What is `result`?<br><br>`directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]`<br>`x, y = (0, 0)`<br>`result = [(x + dx, y + dy) for dx, dy in directions if 0 <= x + dx < 4 and 0 <= y + dy < 3]` | `[(1, 0), (0, 1)]`, a list of tuples. |
| What is `result`?<br><br>`grid = [0, 1, 0, 0, 2, 0, 1, 0, 0, 0, 1, 2]`<br>`result = grid[index_of(1, 2, 4)]` | `1`, an integer. |
| What is `world`?<br><br>`world = {}`<br>`world[(2, 1)] = 1`<br>`world[(2, 1)] = 2` | `{(2, 1): 2}`, a dictionary. |

---

## Chapter Summary

| Question | Answer |
| :--- | :--- |
| What does a nested list provide? | A visible two-dimensional picture of rows and columns. |
| What does a flat list provide? | One-dimensional storage for a world whose width supplies the row structure. |
| What is the coordinate-to-index formula? | `index = y * width + x`. |
| What is the index-to-coordinate formula? | `x = index % width`; `y = index // width`. |
| What does an explicit gridworld lookup provide? | A visible table of how coordinates correspond to storage positions. |
| What does a neighbor dictionary provide? | A visible table of which coordinates are adjacent. |
| What does a direction list and bounds function provide? | A reusable rule for generating valid neighbors in any grid size. |
| What is a dense world? | A representation that stores every cell. |
| What is a sparse world? | A representation that stores only selected cells and supplies a default for missing locations. |
| What is the central idea? | A two-dimensional world can be represented clearly with one-dimensional data, explicit mappings, and carefully named spatial relationships. |

---

**==> The picture may be two-dimensional. The representation is a choice. <==**
