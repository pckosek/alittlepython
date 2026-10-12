# Chapter 13: Procedural Generation

In Chapter 12, small functions learned how to work together.

A function could transform a value. Another could inspect a world. Another could update its state.

Together, those pieces could form a simulation.

Now we ask:

> Can a program create something new from a small set of rules?

The answer is procedural generation.

Procedural generation means using rules, parameters, and sometimes controlled randomness to create a result.

We will begin with repeated values, lists, and strings. Then we will add variation, constraints, coordinates, and finally a generated world.

```text
A recipe can produce one result.
A procedure can produce many related results.
```

---

## Part 13.1 - Repetition Is Already a Procedure

Consider a list that contains the same value several times.

```python
items = [0, 0, 0, 0, 0]
```

| Question | Answer |
| :--- | :--- |
| True or false: the list contains five values. | **True.** There are five zeroes. |
| Does the code describe each zero separately? | Yes. Each value is written by hand. |
| Is there a visible pattern? | Yes. Every value is the same. |
| What would be inconvenient about making a list of one hundred zeroes by hand? | It would be long, repetitive, and easy to miscount. |
| What kind of process could make the list for us? | A repeated procedure. |
| What is the first idea behind procedural generation? | Use a rule to create repeated structure. |

### Fundamental Questions About Repeated Structure

| Question | Answer |
| :--- | :--- |
| What is repetition? | Applying the same action more than once. |
| Does repetition require randomness? | No. A repeated rule can be completely predictable. |
| Can a predictable process still be procedural generation? | Yes. It generates a result from a procedure. |
| Why begin with repeated zeroes? | The rule is visible before variation makes the result complicated. |

---

## Part 13.2 - A Loop Can Build a List

A loop can create the same list while making the repetition explicit.

```python
items = []

for _ in range(5):
    items.append(0)
```

| Question | Answer |
| :--- | :--- |
| True or false: items begins as an empty list. | **True.** No values have been added yet. |
| How many times does the loop body run? | Five times. |
| What does append(0) do? | It adds one zero to the end of the list. |
| What is items after the loop? | [0, 0, 0, 0, 0]. |
| What controls the length of the list? | The argument to range. |
| What controls the value added each time? | The expression inside append. |

### Fundamental Questions About Procedure

| Question | Answer |
| :--- | :--- |
| What does the loop provide? | Repetition. |
| What does append provide? | Growth of the result. |
| What does the value inside append provide? | The rule for each generated member. |
| What would change a list of five values into a list of twelve values? | Change range(5) to range(12). |
| What would change the repeated value from 0 to 1? | Change append(0) to append(1). |
| What is now adjustable? | The size and the generated value. |

---

## Part 13.3 - The Generated Value Can Depend on Position

The result does not have to contain the same value each time.

```python
numbers = []

for number in range(5):
    numbers.append(number)
```

| Question | Answer |
| :--- | :--- |
| True or false: the loop variable changes on each iteration. | **True.** It takes the values 0, 1, 2, 3, and 4. |
| What is appended during the first iteration? | 0. |
| What is appended during the fourth iteration? | 3. |
| What is numbers after the loop? | [0, 1, 2, 3, 4]. |
| What does the generated value depend on? | The current position in the loop. |
| What has changed from the previous example? | The procedure now uses information about the current iteration. |

### Fundamental Questions About Position

| Question | Answer |
| :--- | :--- |
| What is a position? | The current place in a sequence or iteration. |
| Can position be used to generate a value? | Yes. |
| What does number represent in this loop? | The current generated position. |
| What kind of pattern can position create? | Counting, spacing, gradients, coordinates, and other structured variation. |
| Why is position important for images and worlds? | A location often determines the state generated there. |

---

## Part 13.4 - Expressions Create Patterns

The generated value can be calculated from the position.

```python
squares = []

for number in range(5):
    squares.append(number * number)
```

| Question | Answer |
| :--- | :--- |
| True or false: number * number changes on different iterations. | **True.** It uses the current value of number. |
| What is appended when number is 0? | 0. |
| What is appended when number is 3? | 9. |
| What is squares after the loop? | [0, 1, 4, 9, 16]. |
| What does the loop generate? | A list of square numbers. |
| Is the result random? | No. It is determined completely by the rule. |

### Fundamental Questions About Generated Patterns

| Question | Answer |
| :--- | :--- |
| What does the loop vary? | The input position. |
| What does the expression transform? | The position into a generated value. |
| What does the resulting list contain? | The sequence of transformed values. |
| Can a small expression produce a large pattern? | Yes. Repetition gives the expression many opportunities to work. |
| What is a procedural pattern? | A visible structure produced by repeatedly applying a rule. |

---

## Part 13.5 - List Comprehensions as Compact Procedures

The same procedure can be written as a list comprehension.

```python
squares = [number * number for number in range(5)]
```

| Question | Answer |
| :--- | :--- |
| True or false: this produces the same list as the previous loop. | **True.** It produces [0, 1, 4, 9, 16]. |
| What is the value expression? | number * number. |
| What provides the candidate values? | for number in range(5). |
| Where does each calculated value go? | Into the resulting list. |
| Is this a new idea or a compact form of an old idea? | It is a compact form of the loop-and-append pattern. |

> ### Law of Compact Generation
> Every list comprehension is a loop that builds and appends — choose the compact form when the rule is short, and the explicit loop when the process needs to be inspected or debugged.

| Which form may be easier to debug one step at a time? | The ordinary loop. |

### Fundamental Questions About Compact Generation

| Question | Answer |
| :--- | :--- |
| What does a list comprehension express? | Build a list by applying an expression to each value in an iterable. |
| Does a comprehension eliminate the underlying repetition? | No. It expresses the repetition more compactly. |
| When is a comprehension helpful? | When the rule is short and the resulting list is easy to understand. |
| When is an ordinary loop better? | When the process needs several steps, debugging, or explanation. |
| Is concise always the same as clear? | No. Clarity is the goal. |

---

## Part 13.6 - Strings Can Be Generated Too

Procedural generation does not require numbers.

```python
letters = ''

for letter in 'PYTHON':
    letters += letter + '-'
```

| Question | Answer |
| :--- | :--- |
| True or false: the loop visits one character at a time. | **True.** A string can be iterated over. |
| What is added after the first character? | P-. |
| What is added after the second character? | Y-. |
| What is letters after the loop? | P-Y-T-H-O-N-. |
| What does the procedure insert between characters? | A hyphen. |
| What kind of thing has been generated? | A new string with a repeated pattern. |

### Fundamental Questions About Textual Generation

| Question | Answer |
| :--- | :--- |
| Can a procedure generate text? | Yes. |
| What can control the generated text? | Characters, positions, words, conditions, or data from another collection. |
| Could a similar procedure create a row of a grid? | Yes. Each character could represent one cell state. |
| Why are strings useful early examples? | Their output is visible immediately when printed. |

---

## Part 13.7 - Parameters Turn One Pattern into Many

A function can give a procedure adjustable inputs.

```python
def repeated_value(value, count):
    result = []

    for _ in range(count):
        result.append(value)

    return result
```

| Question | Answer |
| :--- | :--- |
| What does value control? | What gets added to the list. |
| What does count control? | How many times it gets added. |
| What is repeated_value(0, 4)? | [0, 0, 0, 0]. |
| What is repeated_value('x', 3)? | ['x', 'x', 'x']. |
| What is repeated_value(7, 0)? | []. The loop runs zero times. |
| What has the function made possible? | One procedure can produce many related results. |

### Fundamental Questions About Parameterized Generation

| Question | Answer |
| :--- | :--- |
| What is a parameter? | An input that lets a function vary its behavior or result. |
| What does count parameterize? | Size. |
| What does value parameterize? | Content. |
| Why is a parameter better than editing the function body each time? | The same rule can be reused with different inputs. |
| What is the relationship between a recipe and a parameter? | The recipe stays stable while the ingredients can change. |

---

## Part 13.8 - Generate from a Rule

Instead of passing a value to repeat, pass a rule that creates each value.

```python
def generate_numbers(count, rule):
    result = []

    for number in range(count):
        result.append(rule(number))

    return result

def square(number):
    return number * number
```

| Question | Answer |
| :--- | :--- |
| True or false: rule can be a function. | **True.** square can be passed as an argument. |
| What does generate_numbers control? | How many positions are generated and when the rule is called. |
| What does square control? | The value produced for each position. |
| What is generate_numbers(5, square)? | [0, 1, 4, 9, 16]. |
| What happens if a different rule is supplied? | The same generation process produces a different pattern. |
| What concept from Chapter 12 is being reused? | A function can receive behavior as an argument. |

### Fundamental Questions About Rule-Based Generation

| Question | Answer |
| :--- | :--- |
| What does a generator need? | A collection of positions and a rule for producing each result. |
| What does the position provide? | Context for the rule. |
| What does the rule provide? | The generated value. |
| Why is this more powerful than repeating one value? | Each position can produce a meaningful result. |

> ### Law of Rule-Based Generation
> A generator needs two things only: a collection of positions to visit, and a rule that turns each position into a value — everything else is repetition.

| What is the generator independent of? | The particular rule used to calculate each value. |

---

## Part 13.9 - Controlled Variation

A procedural rule can produce variation without using randomness.

```python
def alternating(count):
    result = []

    for position in range(count):
        if position % 2 == 0:
            result.append(0)
        else:
            result.append(1)

    return result
```

| Question | Answer |
| :--- | :--- |
| True or false: alternating(6) produces [0, 1, 0, 1, 0, 1]. | **True.** Even positions receive 0; odd positions receive 1. |
| What does % 2 reveal? | Whether a number is even or odd. |
| What controls the change between 0 and 1? | The position's parity. |
| Is this pattern random? | No. The same input always produces the same output. |
| What kind of variation does it have? | Structured variation. |
| Why is structured variation useful? | It creates visible patterns that can be predicted, tested, and modified. |

### Fundamental Questions About Structure Before Randomness

| Question | Answer |
| :--- | :--- |
| Must variation be random? | No. A rule can create variation deterministically. |
| Why begin with deterministic variation? | It makes the relationship between rule and result visible. |
| What does randomness add later? | Uncertainty and variety within boundaries. |
| What should a designer decide before adding randomness? | What structure and constraints the result should have. |

> ### Law of Structure Before Randomness
> Design the shape and the constraints of a generated result before adding randomness to it — randomness should vary a structure, not replace the need for one.

---

## Part 13.10 - Randomness as an Ingredient

Chapter 8 introduced random values and choices.

Here, randomness becomes one ingredient in a larger procedure.

```python
import random

def random_binary(count):
    result = []

    for _ in range(count):
        result.append(random.choice([0, 1]))

    return result
```

| Question | Answer |
| :--- | :--- |
| True or false: random_binary(6) always returns the same list. | **False.** Each choice can vary. |
| What values can appear in the result? | Only 0 and 1. |
| What controls the length? | count. |
| What controls the possible values? | The choices [0, 1]. |
| Is the result completely unconstrained? | No. It varies within a specified set of choices. |
| What does the procedure provide even though the values vary? | A predictable shape: a list of the requested length. |

### Fundamental Questions About Bounded Randomness

| Question | Answer |
| :--- | :--- |
| Does randomness remove all structure? | No. The procedure can still control size, type, range, and placement. |
| What is a random choice? | One variable decision made within a supplied collection. |
| What is a random procedure? | A repeated process that makes one or more controlled random decisions. |
| Why specify the choices explicitly? | The result should vary without becoming meaningless. |
| What should be tested even when the exact result varies? | Invariants such as length, value range, and data type. |

---

## Part 13.11 - Reproducible Generation

A generated result is easier to understand when it can be recreated.

```python
import random

random.seed(4)
first = random_binary(6)

random.seed(4)
second = random_binary(6)
```

| Question | Answer |
| :--- | :--- |
| True or false: first and second contain the same sequence. | **True.** The same seed and the same operations reproduce the same choices. |
| Does the seed make the procedure stop being random in ordinary use? | No. It makes one sequence reproducible. |
| Why is reproducibility useful? | A surprising result can be recreated and investigated. |
| What would change the result? | A different seed or a different sequence of random calls. |
| What should a procedural generator expose for debugging? | Its seed and important parameters. |
| What does reproducible generation provide? | Variation that can still be studied and shared. |

### Fundamental Questions About Reproducible Variation

| Question | Answer |
| :--- | :--- |
| Is a random result necessarily impossible to repeat? | No. A seeded pseudo-random process can be repeated. |
| What is an invariant? | A property that remains true across all valid generated results. |
| Is the exact list an invariant in an unseeded generator? | No. Its length and allowed values may be invariant while its contents vary. |
| Why save the seed with a generated world? | The same world can be reconstructed or debugged later. |

---

## Part 13.12 - Generate, Then Test

A generated candidate may not be acceptable.

```python
def is_even(number):
    return number % 2 == 0

candidates = [number for number in range(10)]
accepted = [number for number in candidates if is_even(number)]
```

| Question | Answer |
| :--- | :--- |
| What values are in candidates? | 0 through 9. |
| What does is_even answer? | Whether its argument is divisible by 2. |
| What values are in accepted? | 0, 2, 4, 6, and 8. |
| What role does the predicate play? | It decides whether a candidate is accepted. |
| Does the procedure generate every candidate into the final result? | No. It filters the candidates. |
| What two stages are visible? | Candidate generation and acceptance testing. |

### Fundamental Questions About Constrained Generation

| Question | Answer |
| :--- | :--- |
| What is a candidate? | A possible result that has not yet been accepted. |
| What is a constraint? | A rule that determines whether a candidate is allowed. |
| What happens to a rejected candidate? | It is left out of the final result. |
| Why separate generation from testing? | The source of possibilities and the definition of validity are different ideas. |
| Can a constraint be a function? | Yes. A predicate can be passed into a generator. |

---

## Part 13.13 - A Generator with a Predicate

The generation and filtering process can be named.

```python
def generate_if(values, predicate):
    result = []

    for value in values:
        if predicate(value):
            result.append(value)

    return result
```

| Question | Answer |
| :--- | :--- |
| True or false: predicate is a function that returns a Boolean. | **True.** It decides whether each value is accepted. |
| What does generate_if control? | The traversal and construction of the result. |
| What does predicate control? | The acceptance rule. |
| What is generate_if(range(10), is_even)? | A list containing 0, 2, 4, 6, and 8. |
| Could a different predicate be supplied? | Yes. |
| What does this function separate? | The mechanism of filtering from the meaning of acceptance. |

### Fundamental Questions About Generative Filters

| Question | Answer |
| :--- | :--- |
| What does the iterable provide? | The candidates. |
| What does the predicate provide? | The constraint. |
| What does the result contain? | Candidates that satisfy the predicate. |
| Is filtering the same as generating? | It is one stage of generation: the selection stage. |
| What larger process is beginning to appear? | Generate possibilities, test them, and retain the acceptable results. |

---

## Part 13.14 - Coordinates as Generated Values

A gridworld can also be generated from a rule.

For a 4 x 3 world, generate every coordinate.

```python
def coordinates(width, height):
    result = []

    for y in range(height):
        for x in range(width):
            result.append((x, y))

    return result
```

| Question | Answer |
| :--- | :--- |
| True or false: coordinates(4, 3) contains twelve coordinates. | **True.** A 4 x 3 world has 4 * 3 locations. |
| What coordinate is generated first? | (0, 0). |
| What coordinate follows (3, 0)? | (0, 1). |
| What coordinate is generated last? | (3, 2). |
| What does the outer loop control? | The row coordinate y. |
| What does the inner loop control? | The column coordinate x. |
| What pattern does this follow? | Row-major order: complete one row before beginning the next. |

### Fundamental Questions About Spatial Generation

| Question | Answer |
| :--- | :--- |
| What does a coordinate generator produce? | A list of locations. |
| Why is the order important? | It can match the storage order of a flat grid. |
| What does width control? | The number of x positions in each row. |
| What does height control? | The number of y positions. |
| How does this connect to Chapter 9? | The same coordinate order can generate or interpret flat grid data. |

---

## Part 13.15 - Generate a World from a Rule

Now each coordinate can receive a value.

```python
def checkerboard(width, height):
    grid = []

    for y in range(height):
        for x in range(width):
            grid.append((x + y) % 2)

    return grid
```

| Question | Answer |
| :--- | :--- |
| True or false: checkerboard(4, 3) returns twelve values. | **True.** One value is generated for each coordinate. |
| What is the value at (0, 0)? | 0. |
| What is the value at (1, 0)? | 1. |
| What is the value at (1, 1)? | 0. |
| What does (x + y) % 2 determine? | Whether the coordinate belongs to one of two alternating classes. |
| Does the function store coordinates in the list? | No. It stores the generated state values in row-major order. |
| What does this function generate? | A flat grid whose values follow a spatial rule. |

### Fundamental Questions About World Generation

| Question | Answer |
| :--- | :--- |
| What does a world generator need? | Dimensions and a rule for assigning a state to each location. |
| What is the input to the rule? | Usually a coordinate, or information derived from one. |
| What is the output of the rule? | The state stored at that coordinate. |
| Why is a flat list sufficient here? | The dimensions and row-major order preserve the spatial interpretation. |
| What is the central transformation? | Coordinates become states. |

---

## Part 13.16 - A Pixel-Ready Generator

The same pattern can produce values suitable for an image.

```python
def horizontal_gradient(width, height):
    pixels = []

    for y in range(height):
        for x in range(width):
            pixels.append(x)

    return pixels
```

| Question | Answer |
| :--- | :--- |
| True or false: every row receives the same sequence of values. | **True.** The value depends on x, not y. |
| What is the value at the left edge? | 0. |
| What happens as x increases? | The generated value increases. |
| What would make a vertical gradient? | Use y instead of x. |
| What does the function return? | A flat list of pixel values in row-major order. |
| Why is this a bridge to PIL? | PIL can accept a flat list of grayscale pixel values with putdata. |

### Fundamental Questions About Coordinate-Dependent Generation

| Question | Answer |
| :--- | :--- |
| What does a horizontal rule depend on? | The column coordinate. |
| What does a vertical rule depend on? | The row coordinate. |
| Can a rule depend on both? | Yes. |
| What might a rule based on both coordinates create? | A checkerboard, a diagonal, a radial pattern, or a more complex field. |
| What is a generated image at this level? | A grid of values produced by a coordinate rule. |

---

## Part 13.17 - A Procedural World with Randomness and Rules

A useful generator often combines structure with variation.

```python
import random

def random_world(width, height, seed):
    random.seed(seed)
    grid = []

    for y in range(height):
        for x in range(width):
            if random.random() < 0.25:
                grid.append(1)
            else:
                grid.append(0)

    return grid
```

| Question | Answer |
| :--- | :--- |
| True or false: the result can vary when the seed changes. | **True.** Different seeds produce different random sequences. |
| True or false: every cell is still either 0 or 1. | **True.** The rule limits the possible values. |
| What is the approximate chance of a 1? | About 25% for each independent decision. |
| Does the generator guarantee exactly 25% ones? | No. Probability describes the tendency, not an exact total for one small world. |
| What does the loop guarantee? | Exactly width * height generated cells. |
| What does the seed guarantee? | The same seed reproduces the same generated world. |

### Fundamental Questions About Structured Random Worlds

| Question | Answer |
| :--- | :--- |
| What does the rule provide? | The possible states and the probability of choosing one. |
| What does the grid structure provide? | The dimensions and storage order. |
| What does the seed provide? | Reproducibility. |
| What does randomness provide? | Variation between generated worlds. |
| Why is this more useful than random values without structure? | Every random choice has a location, a purpose, and a valid range. |

---

## Part 13.18 - Generation as a Design Choice

A procedural generator is not automatically good because it is automatic.

| Question | Answer |
| :--- | :--- |
| True or false: a generated result should be judged only by whether the code runs. | **False.** The result should also satisfy the intended design. |
| What should a generator's invariants describe? | Properties every valid result must have. |
| What might an invariant for a 4 x 3 grid be? | It must contain exactly twelve cells. |
| What might an invariant for a binary grid be? | Every cell must be either 0 or 1. |
| What might an invariant for a world with a starting point be? | The starting point must be in bounds and reachable according to the rules. |
| Why do constraints matter? | They turn arbitrary variation into purposeful variation. |

### Fundamental Questions About Procedural Design

| Question | Answer |
| :--- | :--- |
| Is procedural generation the same as pressing a random button? | No. It is a designed process with rules and boundaries. |
| What makes variation useful? | It remains inside a meaningful structure. |
| What should a generator expose? | Its parameters, rules, constraints, and—when relevant—its seed. |
| Can a small generator produce poor results? | Yes. A procedure can be valid Python and still make bad artifacts. |

> ### Law of Procedural Design
> Procedural generation is not the absence of design — it is the design of a process, with its own invariants, that produces many valid results instead of one.

| What is the designer's role? | Decide what kinds of variation are useful before deciding how to produce them. |

---

## Part 13.19 - Predict the Result

Do not run these immediately. Predict the final value and the data type.

| Question | Answer |
| :--- | :--- |
| What is result?<br><br>result = [number + 1 for number in range(4)] | [1, 2, 3, 4], a list. |
| What is result?<br><br>result = ''.join(['A', 'B', 'C']) | 'ABC', a string. |
| What is result?<br><br>def square(number):<br>&emsp;return number * number<br><br>result = [square(number) for number in range(3)] | [0, 1, 4], a list. |
| What is result?<br><br>def alternating(count):<br>&emsp;return [position % 2 for position in range(count)]<br><br>result = alternating(5) | [0, 1, 0, 1, 0], a list. |
| What is result?<br><br>def coordinates(width, height):<br>&emsp;return [(x, y) for y in range(height) for x in range(width)]<br><br>result = coordinates(4, 3) | A list of twelve coordinate tuples, beginning with (0, 0) and ending with (3, 2). |
| What is result?<br><br>def checkerboard(width, height):<br>&emsp;return [(x + y) % 2 for y in range(height) for x in range(width)]<br><br>result = checkerboard(4, 2) | [0, 1, 0, 1, 1, 0, 1, 0], a list. |
| What is result?<br><br>values = [1, 2, 3, 4]<br>result = [value for value in values if value % 2 == 0] | [2, 4], a list. |
| What is result?<br><br>def make_text(count):<br>&emsp;return ''.join(['x' for _ in range(count)])<br><br>result = make_text(4) | 'xxxx', a string. |
| What property is guaranteed?<br><br>grid = random_world(4, 3, 8) | grid has length 12, and every value is either 0 or 1. |
| What is the larger process?<br><br>generate -> test -> keep | Produce candidates, test them against a rule, and retain the acceptable results. |

---

## Chapter Summary

| Question | Answer |
| :--- | :--- |
| What is procedural generation? | Creating a result by applying rules, parameters, and sometimes controlled randomness. |
| What does repetition provide? | A way to build a result without writing every element by hand. |
| What does position provide? | Context that can determine the generated value. |
| What do parameters provide? | Adjustable size, content, thresholds, and behavior. |
| What does a predicate provide? | A test that accepts or rejects a generated candidate. |
| Why use seeds? | To make random generation reproducible. |
| What does a coordinate generator produce? | Locations that can be used to build grids and worlds. |
| What does a world generator do? | It assigns a state to every coordinate according to a rule. |
| What makes a procedural result useful? | Structure, meaningful variation, and constraints. |
| What is the central idea? | Procedural generation is not the absence of design; it is the design of a process that produces many valid results. |

---

**==> Do not write every result. Write the rule that can produce the results. <==**
