# Chapter 12: Composition

In Chapter 11, a world learned how to survive the program that created it.

We could save it. We could load it. We could reconstruct the lists, dictionaries, tuples, and sets that gave it meaning.

But a world that only sits on a disk is not yet a simulation.

Now we ask:

> How can small pieces become a system?

The answer is composition.

Composition means building a larger process by connecting smaller processes.

One function makes a coordinate.

Another checks it.

Another reads the world.

Another decides what should happen next.

The system is the way these pieces work together.

```text
Small functions provide the pieces.
Composition provides the behavior.
```

---

## Part 12.1 - One Function, One Job

A function is easier to use when its job is clear.

```python
def double(number):
    return number * 2
```

| Question | Answer |
| :--- | :--- |
| True or false: \`double\` has one obvious job. | **True.** It returns twice its input. |
| What is \`double(4)\`? | \`8\`. |
| Does \`double\` need to know where the number came from? | No. It only needs the number it receives. |
| Does \`double\` need to print the result? | No. It returns the result to whoever called it. |
| Why is that useful? | Another function can decide what to do with the returned value. |
| What does this function provide? | One small transformation. |

### Fundamental Questions About One Job

| Question | Answer |
| :--- | :--- |
| What is a function's job? | The process its name promises to perform. |
| Is a function easier to compose when its job is narrow? | Yes. A small, predictable result can become the input to another step. |
| Does “one job” mean a function can contain only one line? | No. It means the function has one coherent purpose. |
| What makes a function a useful piece? | A clear input, a clear output, and a clear responsibility. |

---

## Part 12.2 - The Result of One Function Becomes the Input to Another

Two functions can be connected.

```python
def double(number):
    return number * 2

def add_one(number):
    return number + 1
```

| Question | Answer |
| :--- | :--- |
| True or false: both functions accept a number and return a number. | **True.** They have compatible inputs and outputs. |
| What is \`double(3)\`? | \`6\`. |
| What is \`add_one(double(3))\`? | \`7\`. First \`double(3)\` becomes \`6\`; then \`add_one(6)\` becomes \`7\`. |
| Which function runs first? | \`double\`, because its result is needed before \`add_one\` can run. |
| What does the inner call provide? | The input to the outer call. |
| What does this expression represent? | A two-step pipeline. |

### Fundamental Questions About Compatible Pieces

| Question | Answer |
| :--- | :--- |
| Can any two functions be composed directly? | No. The output of one must make sense as the input of the next. |
| Is a number a useful output for a function expecting a number? | Yes. |
| Is a list automatically a useful output for a function expecting a filename? | No. The types and meanings do not match. |
| What does composition require besides syntax? | Compatible meanings. |
| Why does the nested expression work? | Each function receives the kind of value the next function expects. |

> ### Law of Compatible Pieces
> Two functions compose only when the output of the first is a meaningful input to the second — matching syntax is not enough; the meanings have to line up too.

---

## Part 12.3 - Naming the Stages

Nested calls are compact, but named intermediate values can make the process visible.

```python
number = 3
doubled = double(number)
answer = add_one(doubled)
```

| Question | Answer |
| :--- | :--- |
| What is \`number\`? | \`3\`. |
| What is \`doubled\`? | \`6\`. |
| What is \`answer\`? | \`7\`. |
| Does naming \`doubled\` change the result? | No. It exposes an intermediate result. |
| Why might named stages be better while debugging? | The program can inspect each stage separately. |
| Is this still composition? | Yes. The output of one function still becomes the input of the next. |

### Fundamental Questions About Visible Steps

| Question | Answer |
| :--- | :--- |
| What does a pipeline contain? | A sequence of transformations. |
| What does a named intermediate value provide? | A place to inspect one stage of the pipeline. |
| When is nesting convenient? | When the expression is short and each step is easy to understand. |
| When are named stages useful? | When the process is long, important, or being debugged. |
| Does composition require one giant expression? | No. Clear variables can make composition easier to read. |

---

## Part 12.4 - A Pipeline for Text

The same idea works with strings.

```python
def remove_period(text):
    return text.rstrip('.')

def make_uppercase(text):
    return text.upper()

sentence = 'hello world.'
clean = remove_period(sentence)
result = make_uppercase(clean)
```

| Question | Answer |
| :--- | :--- |
| What is \`sentence\`? | \`'hello world.'\`. |
| What is \`clean\`? | \`'hello world'\`. |
| What is \`result\`? | \`'HELLO WORLD'\`. |
| What does \`remove_period\` provide to \`make_uppercase\`? | A string with the final period removed. |
| Does \`make_uppercase\` need to know that a period was removed? | No. It receives a string and performs its own job. |
| What does each stage preserve? | A value that is meaningful to the next stage. |

### Fundamental Questions About Transformation Pipelines

| Question | Answer |
| :--- | :--- |
| What is a transformation? | A process that receives a value and returns a related value. |
| Can several transformations be chained? | Yes, when their inputs and outputs fit together. |
| Does a transformation have to change the data type? | No. It may return the same kind of value in a new state. |
| What is the advantage of a pipeline? | Each step can be understood, tested, and replaced independently. |

---

## Part 12.5 - Functions That Inspect a World

Chapter 9 gave us a flat grid and a coordinate-to-index function.

Now we can give world questions names.

```python
def index_of(x, y, width):
    return y * width + x

def in_bounds(x, y, width, height):
    return 0 <= x < width and 0 <= y < height

def cell_at(grid, x, y, width, height):
    if in_bounds(x, y, width, height):
        return grid[index_of(x, y, width)]
    return None
```

| Question | Answer |
| :--- | :--- |
| True or false: \`cell_at\` uses two smaller functions. | **True.** It uses \`in_bounds\` and \`index_of\`. |
| What does \`in_bounds\` answer? | Whether the coordinate belongs to the rectangular world. |
| What does \`index_of\` provide? | The list position for a valid coordinate. |
| What does \`cell_at\` provide? | The state at a coordinate, or \`None\` when the coordinate is outside the world. |
| Why check bounds before indexing? | An invalid coordinate should not be allowed to access unintended list data. |
| What does this show about composition? | A larger function can use smaller functions without repeating their rules. |

### Fundamental Questions About Layered Questions

| Question | Answer |
| :--- | :--- |
| What question comes first in \`cell_at\`? | “Is this coordinate safe?” |
| What question comes second? | “Where is this coordinate stored?” |
| What question comes third? | “What value is stored there?” |
| Why keep these questions in separate functions? | Each function can answer one idea while the larger function describes the complete operation. |
| What happens when a rule changes? | The smaller function can be changed without rewriting every caller. |

> ### Law of Layered Questions
> Separate “is this coordinate safe,” “where is it stored,” and “what value is stored there” into their own functions, so that changing one rule never means rewriting the others.

---

## Part 12.6 - A Function That Changes State

Reading a world is not the same as changing it.

```python
def set_cell(grid, x, y, value, width, height):
    if in_bounds(x, y, width, height):
        grid[index_of(x, y, width)] = value
```

| Question | Answer |
| :--- | :--- |
| True or false: \`set_cell\` changes the list it receives. | **True.** It assigns a new value into \`grid\`. |
| What does \`set_cell\` need to know? | The grid, a coordinate, a new value, the width, and the height. |
| What happens for an out-of-bounds coordinate? | Nothing is changed because the assignment is protected by \`in_bounds\`. |
| Does \`set_cell\` return the updated grid? | No. It changes the existing list in place and returns \`None\`. |
| How is \`set_cell\` different from \`cell_at\`? | \`cell_at\` reads a value; \`set_cell\` changes a value. |
| Why name this operation? | Later code can describe a state change without repeating the indexing details. |

### Fundamental Questions About State Updates

| Question | Answer |
| :--- | :--- |
| What is state? | The information describing the current condition of a system. |
| What is a state update? | A change from the current condition to a new condition. |
| Can a function inspect state without changing it? | Yes. \`cell_at\` is an example. |
| Can a function change state without returning the changed structure? | Yes. \`set_cell\` mutates the list in place. |
| What is the danger of hidden mutation? | A function may change data that other parts of the program were also using. |

---

## Part 12.7 - A Function That Computes a New State

Sometimes it is clearer to calculate a new value before changing the world.

```python
def next_value(value):
    if value == 0:
        return 1
    return 0
```

| Question | Answer |
| :--- | :--- |
| What is \`next_value(0)\`? | \`1\`. |
| What is \`next_value(1)\`? | \`0\`. |
| Does \`next_value\` know where the value came from? | No. It transforms one state value into another. |
| Does \`next_value\` change a grid? | No. It only returns a proposed next value. |
| Why separate the rule from the update? | The rule can be tested independently from the storage operation. |
| What is the difference between a proposal and an update? | A proposal describes what should happen; an update applies it to the world. |

### Fundamental Questions About Decision and Application

| Question | Answer |
| :--- | :--- |
| What does a transition rule compute? | A possible next state. |
| What does a state-update function do? | It places a state into the world. |
| Is it useful to separate them? | Yes. The same rule can be applied to different representations or tested without file or grid access. |
| What does this separation prepare us to build? | A repeated simulation step. |

---

## Part 12.8 - One Simulation Step

Now we can compose reading, deciding, and writing.

```python
def step_cell(grid, x, y, width, height):
    current = cell_at(grid, x, y, width, height)
    if current is not None:
        set_cell(grid, x, y, next_value(current), width, height)
```

| Question | Answer |
| :--- | :--- |
| True or false: \`step_cell\` uses several smaller operations. | **True.** It reads, computes, and writes. |
| What does \`current\` hold? | The state currently stored at \`(x, y)\`, if the coordinate is valid. |
| What does \`next_value(current)\` provide? | The proposed replacement state. |
| What does \`set_cell\` do with that proposal? | It writes the new state into the grid. |
| What happens for an invalid coordinate? | \`cell_at\` returns \`None\`, so the update is skipped. |
| What larger process does \`step_cell\` represent? | One local state transition. |

### Fundamental Questions About a Composed Step

| Question | Answer |
| :--- | :--- |
| What is the order of operations? | Read, decide, then write. |
| Why does the order matter? | The decision must use the old state before the new state replaces it. |
| Is \`step_cell\` one primitive Python operation? | No. It is a meaningful operation composed from smaller functions. |
| What does the name \`step_cell\` hide? | The implementation details of bounds checking, indexing, state calculation, and assignment. |

---

## Part 12.9 - Applying a Step to a World

A local step can be repeated across every coordinate.

```python
def step_world(grid, width, height):
    for y in range(height):
        for x in range(width):
            step_cell(grid, x, y, width, height)
```

| Question | Answer |
| :--- | :--- |
| True or false: \`step_world\` visits every coordinate in the world. | **True.** The loops visit each \`x\` and \`y\` in bounds. |
| What does \`step_world\` delegate to \`step_cell\`? | The behavior at one coordinate. |
| What does \`step_world\` provide? | Repetition across the whole world. |
| What is the relationship between the two functions? | \`step_world\` composes a local rule with a traversal pattern. |
| Is the order of updates important here? | Yes. Because the grid is being changed in place, later cells may observe earlier updates. |
| Does this always produce the same result as updating every cell simultaneously? | No. In-place updates and simultaneous updates can differ. |

### Fundamental Questions About Local and Global Behavior

| Question | Answer |
| :--- | :--- |
| What is local behavior? | What happens at one coordinate. |
| What is global behavior? | What happens when the local rule is applied across the world. |
| What does the loop contribute? | Coverage: it chooses which locations receive the local rule. |
| What does the function contribute? | Meaning: it describes what happens at each chosen location. |
| Why separate them? | A local rule can be reused with a different traversal or a different world. |

---

## Part 12.10 - Simultaneous State Updates

If every cell should use the old world, the next world needs separate storage.

```python
def next_grid(grid, width, height):
    result = grid.copy()

    for y in range(height):
        for x in range(width):
            current = cell_at(grid, x, y, width, height)
            if current is not None:
                next_state = next_value(current)
                set_cell(result, x, y, next_state, width, height)

    return result
```

| Question | Answer |
| :--- | :--- |
| True or false: \`grid\` and \`result\` are different list objects. | **True.** \`grid.copy()\` creates a shallow copy of the list. |
| Which grid does \`cell_at\` read? | The original \`grid\`. |
| Which grid does \`set_cell\` change? | The new \`result\`. |
| Why read from \`grid\` and write to \`result\`? | Every update should be based on the same old state. |
| What does the function return? | A new grid containing the next state. |
| Does this function mutate the original \`grid\`? | No. It leaves the original list unchanged. |

### Fundamental Questions About Old and New State

| Question | Answer |
| :--- | :--- |
| What is the old state? | The world before a simulation step begins. |
| What is the new state? | The world produced by applying the transition rule. |
| Why can one list be dangerous? | An early update can affect the inputs used by later updates. |
| Why use two lists? | One list remains the source of truth while the other is constructed. |

> ### Law of Old and New State
> When every cell's next state should depend on the same old world, read from one grid and write to a separate one — updating a single grid in place lets early changes leak into later calculations.

| What pattern is this? | Read from old state, write to new state, then replace the old state. |

---

## Part 12.11 - Repeating the Simulation

Once one step is named, a loop can run the system repeatedly.

```python
def simulate(grid, width, height, steps):
    for _ in range(steps):
        grid = next_grid(grid, width, height)
    return grid
```

| Question | Answer |
| :--- | :--- |
| What does \`steps\` represent? | The number of simulation steps to perform. |
| What does \`_\` represent? | A loop value that is intentionally not needed. |
| What happens inside each iteration? | The current grid is replaced by its next grid. |
| Does \`simulate\` need to know the details of \`next_grid\`? | No. It uses the function as a named step. |
| What does \`simulate\` return? | The world after the requested number of steps. |
| What is the overall structure? | Repeat a state transition and feed each result into the next iteration. |

### Fundamental Questions About Repeated Composition

| Question | Answer |
| :--- | :--- |
| What is a simulation? | Repeatedly applying rules to changing state. |
| What does one step provide? | One transition from an old world to a new world. |
| What does the loop provide? | Repetition of that transition. |
| What does the return value provide? | The final state of the process. |
| Can the same structure simulate a different world? | Yes, if the transition function and representation fit the new world. |

---

## Part 12.12 - A Pipeline for a Saved World

Persistence and composition can cooperate.

```python
world = load_world('world.json')

grid = world['cells']
width = world['width']
height = world['height']

grid = simulate(grid, width, height, 5)

world['cells'] = grid
save_world('world.json', world)
```

| Question | Answer |
| :--- | :--- |
| What happens first? | The saved world is loaded. |
| What happens next? | Its dimensions and cells are given names. |
| What does \`simulate\` produce? | A grid after five state transitions. |
| Why assign the result back to \`world['cells']\`? | The dictionary must contain the updated cells before saving. |
| What happens last? | The new world is serialized back into the file. |
| What has become a system here? | Loading, transforming, simulating, and saving are connected into one meaningful process. |

### Fundamental Questions About the Full Cycle

| Question | Answer |
| :--- | :--- |
| What is the input to the pipeline? | A saved world file. |
| What is the middle of the pipeline? | A sequence of composed state transitions. |
| What is the output? | A new saved world file containing the updated state. |
| Can each stage be tested separately? | Yes. Loading, simulation, and saving can each be examined on its own. |
| Why is this better than one enormous block of code? | Each stage has a name, a responsibility, and a place where errors can be located. |

---

## Part 12.13 - Functions as Choices

A composed system can accept a function as a parameter.

```python
def apply_to_grid(grid, width, height, rule):
    result = grid.copy()

    for y in range(height):
        for x in range(width):
            current = cell_at(grid, x, y, width, height)
            if current is not None:
                new_value = rule(current)
                set_cell(result, x, y, new_value, width, height)

    return result
```

| Question | Answer |
| :--- | :--- |
| True or false: \`rule\` is a parameter whose value can be a function. | **True.** Functions are values that can be passed into other functions. |
| What does \`apply_to_grid\` decide? | Where the rule is applied and how the old and new grids are managed. |
| What does \`rule\` decide? | How one cell value becomes another. |
| What does this function separate? | Traversal and state storage from the particular transition rule. |
| Could \`next_value\` be passed as \`rule\`? | Yes. |
| What larger idea does this introduce? | A system can be configured by passing behavior as data. |

### Fundamental Questions About Behavior as an Argument

| Question | Answer |
| :--- | :--- |
| Can a function receive another function? | Yes. |
| What does the receiving function control? | When and where the provided function is called. |
| What does the provided function control? | The behavior performed at that moment. |
| Is this the same as copying the function's code into the loop? | No. The behavior remains a separate named piece. |
| Why is this useful? | One traversal can support many rules. |

> ### Law of Behavior as an Argument
> A function can be passed as an ordinary value — this lets one traversal or one pipeline stay fixed while the rule it applies changes freely.

---

## Part 12.14 - Predict the Result

Do not run these immediately. Predict the final value and the data type.

| Question | Answer |
| :--- | :--- |
| What is \`result\`?<br><br>\`def double(number):\`<br>&emsp;\`return number * 2\`<br><br>\`def add_one(number):\`<br>&emsp;\`return number + 1\`<br><br>\`result = add_one(double(4))\` | \`9\`, an integer. |
| What is \`answer\`?<br><br>\`value = 5\`<br>\`value = double(value)\`<br>\`answer = add_one(value)\` | \`11\`, an integer. |
| What is \`result\`?<br><br>\`def next_value(value):\`<br>&emsp;\`return value + 1\`<br><br>\`result = next_value(3)\` | \`4\`, an integer. |
| What is \`grid\` after the call?<br><br>\`grid = [0, 0, 0, 0]\`<br>\`set_cell(grid, 2, 0, 1, 4, 1)\` | \`[0, 0, 1, 0]\`, a list. |
| What is \`result\`?<br><br>\`grid = [7, 8, 9, 10]\`<br>\`result = cell_at(grid, 1, 0, 4, 1)\` | \`8\`, an integer. |
| What is \`result\`?<br><br>\`grid = [7, 8, 9, 10]\`<br>\`result = cell_at(grid, 4, 0, 4, 1)\` | \`None\`, because \`(4, 0)\` is outside the world. |
| What is \`result\`?<br><br>\`def add_one(number):\`<br>&emsp;\`return number + 1\`<br><br>\`result = apply_to_grid([1, 2, 3], 3, 1, add_one)\` | \`[2, 3, 4]\`, a new list. |
| What is the final value?<br><br>\`value = 1\`<br>\`for _ in range(3):\`<br>&emsp;\`value = double(value)\` | \`8\`, an integer. |
| What is the system's order?<br><br>\`load -> simulate -> save\` | Load the saved state, transform it repeatedly, then save the resulting state. |

---

## Chapter Summary

| Question | Answer |
| :--- | :--- |
| What is composition? | Building a larger process by connecting smaller processes. |
| What is a pipeline? | A sequence in which one stage's output becomes the next stage's input. |
| Why name intermediate stages? | To make a process readable, testable, and debuggable. |
| What is a state update? | A change from the current condition of a system to a new condition. |
| Why separate old and new state? | To ensure that every update is based on the same previous state when simultaneous behavior is intended. |
| What does a simulation loop do? | Repeats a transition and carries each new state into the next step. |
| What does it mean to pass a function as an argument? | Behavior becomes configurable data that another function can call. |
| What does persistence add to composition? | The system can load a state, transform it, and save the result. |
| What is the central idea? | A system is not one enormous operation; it is a collection of small, named pieces whose relationships create larger behavior. |

---

**==> Small functions are the vocabulary. Composition is the sentence. A system is the story they tell together. <==**
