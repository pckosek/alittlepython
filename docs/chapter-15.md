# Chapter 15: Procedural PIL

In Chapter 14, an image became a grid of pixel values.

A grayscale pixel was one number.

An RGB pixel was a tuple.

A coordinate identified a location.

A flat list stored the pixels.

PIL made the result visible.

Now we ask:

> Can a function generate the image itself?

The answer is yes.

A procedural image is produced by a rule.

The rule receives a coordinate.

The rule returns a pixel value.

Repeated across the whole rectangle, that small rule becomes an image.

```text
A coordinate becomes a pixel.
A pixel rule becomes an image.
Several image rules become a visual system.
```

---

## Part 15.1 - One Function Can Describe One Pixel

Begin with a single rule.

```python
def gray_pixel(x, y):
    return 128
```

| Question | Answer |
| :--- | :--- |
| True or false: gray_pixel returns the same value for every coordinate. | **True.** It always returns 128. |
| What is gray_pixel(0, 0)? | 128. |
| What is gray_pixel(50, 20)? | 128. |
| Does this function care about x or y? | No. |
| What kind of image would this rule create? | A solid medium-gray image. |
| What does the function describe? | The pixel value at one coordinate. |

### Fundamental Questions About the Pixel Function

| Question | Answer |
| :--- | :--- |
| What does the input represent? | A location in the image. |
| What does the output represent? | The pixel value at that location. |
| Does the function need to create the image itself? | No. It only needs to answer the pixel question. |
| What does a larger image require? | More coordinates sent through the same rule. |
| What is the first step toward procedural PIL? | Separate the rule for one pixel from the process that visits every pixel. |

---

## Part 15.2 - A General Image Generator

Now give the pixel rule to a function that knows how to build an image.

```python
from PIL import Image

def build_index_lookup(width, height):
    return {
        (x, y): y * width + x
        for y in range(height)
        for x in range(width)
    }

def generate_grayscale(width, height, pixel_at):
    index_of = build_index_lookup(width, height)
    pixels = [0] * (width * height)

    for y in range(height):
        for x in range(width):
            pixels[index_of[(x, y)]] = pixel_at(x, y)

    image = Image.new("L", (width, height))
    image.putdata(pixels)
    return image
```

| Question | Answer |
| :--- | :--- |
| True or false: generate_grayscale visits every coordinate. | **True.** The nested loops cover the rectangle. |
| What does build_index_lookup provide? | A dictionary mapping every coordinate to its position in the flat pixel list, computed once. |
| Why build that lookup once instead of writing `y * width + x` inline? | generate_grayscale will be called many times, with many different pixel rules, over the rest of this chapter — the lookup is built once per call and reused for every one of its pixels. |
| What does pixel_at provide? | One grayscale value for the current coordinate. |
| What does `pixels[index_of[(x, y)]] = pixel_at(x, y)` do that appending would not? | It writes into a slot that already exists, so the list never has to grow while the image is generated. |
| What does Image.new provide? | An empty grayscale image with the requested dimensions. |
| What does putdata do? | It places the generated pixel list into the image. |
| What does the function return? | A complete PIL image. |

Use the generator with the constant rule.

```python
def gray_pixel(x, y):
    return 128

image = generate_grayscale(100, 60, gray_pixel)
image.save("procedural_gray.png")
```

| Question | Answer |
| :--- | :--- |
| What should procedural_gray.png look like? | A solid medium-gray rectangle. |
| What changed from Chapter 14? | The pixel values are now supplied by a function. |
| What did not change? | The image still has dimensions, a mode, and a flat pixel sequence. |
| What does generate_grayscale hide? | The repeated list-building and PIL setup. |
| What can now change independently? | The image-generation mechanism or the pixel rule. |

### Fundamental Questions About the General Generator

| Question | Answer |
| :--- | :--- |
| What does the generator control? | Traversal, storage, image mode, and image creation. |
| What does pixel_at control? | The value produced at each coordinate. |
| Why separate them? | A new visual rule can reuse the same image-making machinery. |
| What is the reusable shape? | Visit each coordinate, ask for a pixel, store the result, and create the image. |

> ### Law of the General Generator
> Separate the rule for one pixel from the machinery that visits every coordinate — the same generator can serve any pixel rule that fits its signature.

---

## Part 15.3 - A Coordinate Can Become Brightness

A horizontal gradient is a direct coordinate rule.

```python
def horizontal_pixel(x, y):
    return x
```

| Question | Answer |
| :--- | :--- |
| True or false: brightness depends on the column. | **True.** The function returns x. |
| What happens as x increases? | The pixel becomes brighter. |
| What does y affect? | Nothing. |
| What pattern appears across each row? | A left-to-right gradient. |
| What pattern appears from row to row? | The same gradient repeats. |

```python
image = generate_grayscale(256, 80, horizontal_pixel)
image.save("procedural_horizontal.png")
```

| Question | Answer |
| :--- | :--- |
| Why is width 256 useful here? | It gives one coordinate for each ordinary grayscale value from 0 through 255. |
| What should the left edge look like? | Black or nearly black. |
| What should the right edge look like? | White. |
| What is the visual meaning of x? | Horizontal position becomes brightness. |
| What is the general pattern? | Location becomes appearance. |

Now change the rule.

```python
def vertical_pixel(x, y):
    return y

image = generate_grayscale(80, 256, vertical_pixel)
image.save("procedural_vertical.png")
```

| Question | Answer |
| :--- | :--- |
| What changed? | The rule returns y instead of x. |
| What should the gradient direction become? | Top to bottom. |
| Does the image generator change? | No. Only the pixel rule changes. |
| What does this demonstrate? | A reusable mechanism can support different visual behaviors. |

### Fundamental Questions About Coordinate-Driven Appearance

| Question | Answer |
| :--- | :--- |
| What does a horizontal rule use? | x. |
| What does a vertical rule use? | y. |
| What does a diagonal rule use? | Some combination of x and y. |
| What does a radial rule use? | Distance from a center point. |
| What does the generator do with these rules? | It turns their answers into visible pixels. |

---

## Part 15.4 - Keep Pixel Values in Range

A pixel rule must produce valid pixel values.

```python
def safe_horizontal_pixel(x, y):
    return min(255, x)
```

| Question | Answer |
| :--- | :--- |
| True or false: safe_horizontal_pixel can return a value greater than 255. | **False.** min limits the result to 255. |
| What happens when x is 40? | The function returns 40. |
| What happens when x is 300? | The function returns 255. |
| Why might this matter? | An image wider than 256 pixels can produce x values outside the grayscale range. |
| What does min provide? | A simple upper bound. |

A more general version can scale the coordinate into the available range.

```python
def gradient_pixel(x, y, width):
    return int(255 * x / (width - 1))
```

| Question | Answer |
| :--- | :--- |
| What happens at x equal to 0? | The result is 0. |
| What happens at the right edge x equal to width - 1? | The result is 255. |
| What does width control? | How the available brightness range is distributed across the image. |
| Why is int used? | A grayscale pixel needs an integer value. |
| What problem must be considered? | A width of 1 would make width - 1 equal to zero. |

### Fundamental Questions About Valid Generation

| Question | Answer |
| :--- | :--- |
| What must a grayscale rule return? | An integer in the valid grayscale range. |
| What must an RGB rule return? | A three-component tuple with valid channel values. |
| What should a generator do with edge cases? | Anticipate them and either handle them or state the required conditions. |
| Is a visually interesting rule automatically a valid pixel rule? | No. It must also produce acceptable data. |

---

## Part 15.5 - A Color Can Depend on a Coordinate

The same idea works with RGB tuples.

```python
def rgb_pixel(x, y):
    return (128, x, 210)
```

| Question | Answer |
| :--- | :--- |
| True or false: the function returns an RGB tuple. | **True.** It returns three channel values. |
| Which channel remains constant? | Red remains 128, and blue remains 210. |
| Which channel depends on x? | Green. |
| What happens as x increases? | The green channel becomes stronger. |
| What kind of pattern does this make? | A color gradient across the image. |

```python
def generate_rgb(width, height, pixel_at):
    index_of = build_index_lookup(width, height)
    pixels = [(0, 0, 0)] * (width * height)

    for y in range(height):
        for x in range(width):
            pixels[index_of[(x, y)]] = pixel_at(x, y)

    image = Image.new("RGB", (width, height))
    image.putdata(pixels)
    return image

image = generate_rgb(256, 80, rgb_pixel)
image.save("procedural_rgb.png")
```

| Question | Answer |
| :--- | :--- |
| What does generate_rgb change from generate_grayscale? | The image mode, the placeholder value, and the expected pixel type. |
| Why does generate_rgb call build_index_lookup again instead of reusing generate_grayscale's? | Each call generates an image of its own width and height, so each needs its own lookup. |
| What does rgb_pixel provide? | A tuple for each coordinate. |
| What should the image show? | A color transition from left to right. |
| What is the reusable pattern? | One generator for RGB images, many possible RGB pixel rules — and both generators share the same index-lookup helper. |

### Fundamental Questions About Channel Rules

| Question | Answer |
| :--- | :--- |
| Can each channel have its own rule? | Yes. |
| Can red depend on x while blue depends on y? | Yes. |
| Can all three channels depend on both coordinates? | Yes. |
| What does a color image provide? | Three coordinated spaces for procedural variation. |
| What should be checked before saving? | Every channel value must fit the image mode's requirements. |

---

## Part 15.6 - A Predicate Can Describe a Shape

A shape can be described by a true-or-false question.

```python
def is_border(x, y, width, height):
    return (
        x == 0
        or y == 0
        or x == width - 1
        or y == height - 1
    )
```

| Question | Answer |
| :--- | :--- |
| True or false: is_border answers a Boolean question. | **True.** It returns True or False. |
| What does True mean? | The coordinate lies on the border. |
| What does False mean? | The coordinate lies inside the border. |
| What information does the predicate need? | The coordinate and the image dimensions. |
| What kind of visual idea is this? | Membership in a shape. |

Now turn that predicate into pixels.

```python
def border_pixel(x, y):
    if is_border(x, y, 100, 60):
        return 255
    return 0

image = generate_grayscale(100, 60, border_pixel)
image.save("procedural_border.png")
```

| Question | Answer |
| :--- | :--- |
| What does a border coordinate become? | White. |
| What does an interior coordinate become? | Black. |
| What does the predicate decide? | Which pixels belong to the shape. |
| What does the pixel rule decide? | How membership appears visually. |
| What larger pattern is appearing? | A shape predicate and a color rule can be composed. |

### Fundamental Questions About Masks

| Question | Answer |
| :--- | :--- |
| What is a mask? | A Boolean description of where a rule applies. |
| What does a mask return? | True for included locations and False for excluded locations. |
| Can one mask create different images? | Yes. True locations could become white, red, transparent, or another value. |
| Why separate the mask from the appearance? | The same shape can be displayed in several ways. |

> ### Law of Masks
> A mask is a Boolean rule that answers only “does this coordinate belong?” — keep that question separate from the question of what color belonging should produce.

---

## Part 15.7 - Distance Creates a Circle

A circle can be described by distance from a center.

```python
def inside_circle(x, y, center_x, center_y, radius):
    distance_squared = (
        (x - center_x) ** 2
        + (y - center_y) ** 2
    )
    return distance_squared <= radius ** 2
```

| Question | Answer |
| :--- | :--- |
| True or false: the function returns True for points inside or on the circle. | **True.** It compares distance with radius. |
| Why use distance squared? | Squaring avoids needing a square root while preserving the comparison. |
| What does center_x describe? | The horizontal location of the circle's center. |
| What does center_y describe? | The vertical location of the center. |
| What does radius describe? | How far the circle extends. |

```python
def circle_pixel(x, y):
    if inside_circle(x, y, 50, 30, 20):
        return 255
    return 0

image = generate_grayscale(100, 60, circle_pixel)
image.save("procedural_circle.png")
```

| Question | Answer |
| :--- | :--- |
| What should the image contain? | A white circle on a black background. |
| What does every pixel ask? | Is this coordinate inside the circle? |
| What does the answer become? | A white or black pixel. |
| What can change the circle's appearance? | Its center, radius, or the values assigned inside and outside. |
| What is the important conceptual step? | Geometry becomes a Boolean rule, and the Boolean rule becomes pixels. |

### Fundamental Questions About Geometric Generation

| Question | Answer |
| :--- | :--- |
| What does a geometric predicate describe? | A spatial region. |
| What does the image generator do with that region? | It visits every coordinate and asks whether the coordinate belongs. |
| Can the same method describe rectangles? | Yes. |
| Can it describe diagonal bands? | Yes. |
| Can it describe combinations of shapes? | Yes, by combining predicates. |

---

## Part 15.8 - Compose Shapes

Several predicates can become one larger rule. `inside_circle` already exists from Part 15.7 — reuse it rather than writing it again under a new name.

```python
def inside_square(x, y):
    return 20 <= x < 80 and 15 <= y < 45

def shape_pixel(x, y):
    if inside_square(x, y) or inside_circle(x, y, 50, 30, 15):
        return 255
    return 0
```

| Question | Answer |
| :--- | :--- |
| True or false: shape_pixel includes a coordinate inside either shape. | **True.** The condition uses or. |
| What happens where the shapes overlap? | The coordinate is still simply included in the mask. |
| Why does inside_circle still take center_x, center_y, and radius here? | It is the same function defined in Part 15.7 — reusing it means reusing its signature too. |
| What would go wrong if a second, differently-shaped inside_circle(x, y) were defined here? | It would redefine the name from Part 15.7, so any earlier code that still expects the five-argument version would break. |
| What does each helper contribute? | One geometric idea. |
| What does shape_pixel contribute? | The composition of those ideas into one visual rule. |
| What is this an example of? | Small predicates becoming a larger procedural image. |

### Fundamental Questions About Shape Composition

| Question | Answer |
| :--- | :--- |
| What does or mean spatially? | Include a coordinate if it belongs to at least one shape. |
| What would and mean? | Include a coordinate only if it belongs to both shapes. |
| What would not mean? | Include a coordinate outside a shape. |
| Why use helper functions? | Their names make the visual rule easier to read. |
| What larger system is being built? | A collection of geometric predicates feeding one pixel rule. |

> ### Law of Reused Names
> A function should keep one signature everywhere it is called from; redefining an existing name with a different signature silently breaks any earlier code that still expects the original one.

---

## Part 15.9 - Different Regions Can Have Different Colors

A mask does not have to produce only black and white.

```python
def color_pixel(x, y):
    if x < 50:
        return (255, 80, 80)
    return (80, 80, 255)

image = generate_rgb(100, 60, color_pixel)
image.save("procedural_split_color.png")
```

| Question | Answer |
| :--- | :--- |
| True or false: the image is divided by a vertical boundary. | **True.** The rule checks x. |
| What color appears on the left? | Light red. |
| What color appears on the right? | Light blue. |
| What determines the region? | The condition x < 50. |
| What determines the appearance? | The RGB tuples returned by the two branches. |

Now combine a shape with a background.

```python
def circle_color_pixel(x, y):
    if inside_circle(x, y, 50, 30, 20):
        return (255, 220, 80)
    return (20, 40, 90)

image = generate_rgb(100, 60, circle_color_pixel)
image.save("procedural_colored_circle.png")
```

| Question | Answer |
| :--- | :--- |
| What is the background color? | Dark blue. |
| What is the circle color? | Yellow. |
| What does the circle predicate decide? | Which color rule applies. |
| What has been separated? | Geometry and appearance. |
| Why is this useful? | The same shape can receive different palettes without rewriting its geometry. |

### Fundamental Questions About Layered Appearance

| Question | Answer |
| :--- | :--- |
| What does a background rule provide? | A default pixel value. |
| What does a shape rule provide? | A condition for replacing the default. |
| What does a palette provide? | The colors assigned to different regions or states. |
| What is a layer? | A visual rule applied over an existing result. |
| What does composition allow? | Several simple visual decisions to cooperate. |

---

## Part 15.10 - Add Controlled Randomness

A procedural image can include random variation.

```python
import random

def noise_pixel(x, y):
    if random.random() < 0.5:
        return 0
    return 255
```

| Question | Answer |
| :--- | :--- |
| True or false: the same coordinate can receive different values on different runs. | **True.** The rule uses randomness. |
| What values can the function return? | 0 or 255. |
| What kind of image might this produce? | Black-and-white noise. |
| Is there still structure? | Yes. Every coordinate receives one valid grayscale value. |
| Is the exact image reproducible yet? | Not necessarily. A seed has not been chosen. |

Add a seed outside the pixel rule.

```python
random.seed(12)
image = generate_grayscale(100, 60, noise_pixel)
image.save("procedural_noise.png")
```

| Question | Answer |
| :--- | :--- |
| What does the seed control? | The sequence of random choices. |
| What happens when the same seed and code are used again? | The same image can be generated again. |
| What happens when the seed changes? | A different noise image is produced. |
| Why should the seed not be reset inside noise_pixel? | Resetting it for every pixel would destroy the intended sequence of variation. |

> ### Law of Procedural Noise
> Seed the random generator once, outside the pixel rule — reseeding inside a per-pixel function destroys the sequence of variation the seed was meant to control.

| What does this combine? | Coordinate traversal, pixel rules, randomness, and persistence. |

### Fundamental Questions About Procedural Noise

| Question | Answer |
| :--- | :--- |
| What is noise? | Variation that changes from location to location without following a simple smooth pattern. |
| Does noise have to be completely uncontrolled? | No. Its range, distribution, and seed can be controlled. |
| What should a noisy image still guarantee? | Valid dimensions, valid pixel values, and reproducibility when desired. |
| Why is noise useful? | It can provide texture, variation, and irregularity. |

---

## Part 15.11 - A Complete Procedural Image Function

The pieces can be gathered into one named process.

```python
def make_sun_image(width, height):
    def pixel_at(x, y):
        if inside_circle(x, y, width // 2, height // 2, height // 3):
            return (255, 220, 80)
        return (20, 40, 90)

    return generate_rgb(width, height, pixel_at)
```

| Question | Answer |
| :--- | :--- |
| True or false: make_sun_image returns a PIL image. | **True.** generate_rgb returns the image. |
| What does the inner function describe? | The pixel color at one coordinate. |
| What does the circle predicate describe? | The sun's shape. |
| What does the outer function provide? | The image dimensions and the overall artifact. |
| What happens when the dimensions change? | The sun's center and radius are calculated relative to the new size. |
| What does this show about composition? | A complete artifact can be built from small rules with distinct jobs. |

```python
image = make_sun_image(160, 100)
image = image.resize((640, 400), Image.NEAREST)
image.save("procedural_sun.png")
```

### Visual Check: The Procedural Sun

| Question | Answer |
| :--- | :--- |
| What should the image contain? | A yellow circle on a dark blue background. |
| Why enlarge it with nearest-neighbor scaling? | To make the generated structure easy to inspect. |
| Does scaling create the sun? | No. The procedural rules create it; scaling only changes its display size. |
| What parameters could become adjustable? | Width, height, center, radius, background color, and sun color. |

---

## Part 15.12 - Save the Rules and the Result

Chapter 11 taught us to save data.

A procedural artifact can save both the image and the recipe that produced it.

| Question | Answer |
| :--- | :--- |
| True or false: saving only the PNG preserves every generation decision. | **False.** It preserves the visual result, but not necessarily the seed or parameters. |
| What could be saved alongside the image? | The seed, dimensions, colors, radius, and other parameters. |
| Why save the parameters? | The image can be reproduced, modified, or explained later. |
| Is the image file itself enough to regenerate the same process? | No. It contains pixels, not the original Python rules. |
| What is the difference between an artifact and a recipe? | The artifact is the generated image; the recipe is the code and data that produce it. |

```python
import json

settings = {
    "width": 160,
    "height": 100,
    "background": [20, 40, 90],
    "sun": [255, 220, 80],
    "radius": 33,
    "seed": 12
}

with open("sun_settings.json", "w") as file:
    json.dump(settings, file, indent=2)
```

| Question | Answer |
| :--- | :--- |
| Why are the colors stored as lists? | JSON represents arrays as lists. |
| What kind of values are the width and height? | Integers. |
| What does the seed provide? | A reproducible source of variation. |
| What does saving settings add? | A record of how the image was configured. |
| What could a later program do with this file? | Load the settings and regenerate a related image. |

### Fundamental Questions About Procedural Persistence

| Question | Answer |
| :--- | :--- |
| What should be saved for reproducibility? | The generation rules or their name, the parameters, and the random seed when relevant. |
| What should be saved for inspection? | The image itself. |
| Why save both? | The image shows what happened; the settings explain how it happened. |
| What does this connect to? | Persistence, procedural generation, and visual artifacts become one system. |

---

## Part 15.13 - The Procedural Image Pipeline

A complete procedural image project can be described as a pipeline.

```text
choose parameters
        |
define a pixel rule
        |
visit every coordinate
        |
collect pixel values
        |
build the PIL image
        |
inspect, scale, and save
```

| Question | Answer |
| :--- | :--- |
| What happens first? | The designer chooses dimensions, colors, thresholds, and seeds. |
| What happens next? | A rule is written for one coordinate. |
| What does the generator do? | It asks the rule about every coordinate. |
| What is collected? | A flat list of pixel values. |
| What does PIL provide? | The image container and file output. |
| What happens after generation? | The image is inspected, enlarged if useful, and saved. |

### Fundamental Questions About the Pipeline

| Question | Answer |
| :--- | :--- |
| Can each stage be tested separately? | Yes. |
| What could be tested first? | A pixel rule at a few hand-selected coordinates. |
| What could be tested next? | The length and value types of the generated pixel list. |
| What could be tested after that? | The image dimensions, appearance, and saved file. |
| Why use a pipeline? | Problems can be located at the stage where they occur. |

---

## Part 15.14 - Predict the Result

Do not run these immediately. Predict the final value and the data type.

| Question | Answer |
| :--- | :--- |
| What is result?<br><br>def pixel_at(x, y):<br>&emsp;return 0<br><br>result = pixel_at(20, 30) | 0, an integer. |
| What is result?<br><br>def pixel_at(x, y):<br>&emsp;return (x, y, 0)<br><br>result = pixel_at(4, 7) | (4, 7, 0), a tuple. |
| What is result?<br><br>def pixel_at(x, y):<br>&emsp;return 255 if x < 3 else 0<br><br>result = pixel_at(1, 9) | 255, an integer. |
| What is result?<br><br>def pixel_at(x, y):<br>&emsp;return 255 if x < 3 else 0<br><br>result = pixel_at(5, 9) | 0, an integer. |
| What is result?<br><br>pixels = [x for y in range(2) for x in range(4)]<br>result = len(pixels) | 8, an integer. |
| What is result?<br><br>def inside_square(x, y):<br>&emsp;return 1 <= x < 3 and 1 <= y < 3<br><br>result = inside_square(2, 1) | True, a Boolean. |
| What is result?<br><br>def inside_square(x, y):<br>&emsp;return 1 <= x < 3 and 1 <= y < 3<br><br>result = inside_square(3, 1) | False, a Boolean. |
| What is the image mode?<br><br>image = generate_grayscale(80, 60, horizontal_pixel)<br>result = image.mode | "L", a string. |
| What is the larger process?<br><br>coordinate -> pixel rule -> pixel list -> image | Ask for a value at every coordinate, collect the values, and place them into a PIL image. |

---

## Chapter Summary

| Question | Answer |
| :--- | :--- |
| What is a pixel rule? | A function that maps a coordinate to a pixel value. |
| What does a general image generator do? | Visits every coordinate, collects each pixel value, and constructs an image. |
| What is a mask? | A Boolean rule describing where a visual condition is true. |
| What can geometric predicates describe? | Borders, rectangles, circles, bands, and combinations of shapes. |
| What can RGB rules generate? | Color fields, palettes, gradients, and colored shapes. |
| What does randomness add? | Controlled variation and texture. |
| Why use a seed? | To make a random image reproducible. |
| What should be saved for a reproducible artifact? | The image, important parameters, and the seed when relevant. |
| What is the central idea? | Procedural PIL turns coordinate-based rules into visible images through a reusable generation pipeline. |

---

**==> Write the rule for one pixel. Let repetition reveal the image. <==**
