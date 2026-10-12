# Chapter 14: PIL

In Chapter 13, a procedure learned how to generate lists, coordinates, worlds, and values.

Now we need a new kind of world.

> What is an image, if we stop looking at it and start representing it?

An image is not magic.

It is a rectangle.

The rectangle has a width and a height.

Each location in the rectangle has a pixel value.

PIL gives Python tools for creating, inspecting, changing, resizing, and saving those pixels.

We will begin with one pixel.

Then we will make a row of pixels.

Then a whole image.

```text
An image is a grid of values.
PIL gives the grid a visible form.
```

---

## Part 14.1 - A Pixel Has a Value

A grayscale image can represent each pixel with one number.

```python
from PIL import Image

img = Image.new("L", (100, 60), color=128)
img.save("gray_image.png")
```

| Question | Answer |
| :--- | :--- |
| True or false: this code creates an image. | **True.** It creates a new PIL image. |
| What does "L" mean? | A grayscale image with one brightness value per pixel. |
| What does (100, 60) represent? | The width and height of the image. |
| What does 128 represent? | The initial grayscale value for every pixel. |
| What would 0 represent in mode "L"? | Black. |
| What would 255 represent? | White. |
| What would 128 look like? | A medium gray. |

### Visual Check: One Number, One Brightness

Run the example and inspect gray_image.png.

| Question | Answer |
| :--- | :--- |
| What should the entire image look like? | One uniform rectangle of medium gray. |
| Why are there no visible shapes? | Every pixel begins with the same value. |
| What would happen if the color were 0? | The whole image would be black. |
| What would happen if the color were 255? | The whole image would be white. |
| What does this show? | An image can be created by giving every location the same state. |

### Fundamental Questions About Grayscale Pixels

| Question | Answer |
| :--- | :--- |
| How many values describe one grayscale pixel? | One integer. |
| What does that integer describe? | Brightness. |
| What is the usual valid range? | 0 through 255. |
| Is a grayscale pixel a coordinate? | No. A coordinate identifies its location; the pixel value describes its state. |
| What does a grayscale image contain? | A rectangular grid of brightness values. |

---

## Part 14.2 - A Pixel Can Have Channels

Color images need more information than grayscale images.

```python
img = Image.new("RGB", (64, 128), color=(0, 255, 0))
img.save("solid_green.png")
```

| Question | Answer |
| :--- | :--- |
| True or false: this image uses three values for each pixel. | **True.** RGB means red, green, and blue. |
| What does (0, 255, 0) represent? | No red, full green, and no blue: a solid green image. |
| What would (255, 0, 0) represent? | Solid red. |
| What would (0, 0, 255) represent? | Solid blue. |
| What would (255, 255, 0) represent? | Yellow: full red and full green, with no blue. |
| What does each number usually range from? | 0 through 255. |

### Fundamental Questions About Color Channels

| Question | Answer |
| :--- | :--- |
| What is a channel? | One component of a pixel's representation. |
| What does the red channel control? | The amount of red in the pixel. |
| What does the green channel control? | The amount of green. |
| What does the blue channel control? | The amount of blue. |
| What does an RGB image contain? | A rectangular grid of three-value pixel tuples. |

---

## Part 14.3 - Dimensions Belong to the Image

The tuple passed to Image.new gives the image its dimensions.

```python
width, height = 100, 60
img = Image.new("L", (width, height), color=128)
```

| Question | Answer |
| :--- | :--- |
| True or false: width is 100. | **True.** The image has 100 columns. |
| True or false: height is 60. | **True.** The image has 60 rows. |
| How many pixel locations does the image contain? | 100 * 60 = 6000. |
| Why is the total not 100 + 60? | Every row contains 100 pixels, and there are 60 rows. |
| What does img.size provide? | The tuple (100, 60). |
| What does img.width provide? | 100. |
| What does img.height provide? | 60. |

### Fundamental Questions About Image Dimensions

| Question | Answer |
| :--- | :--- |
| What does width describe? | How many columns the image has. |
| What does height describe? | How many rows the image has. |
| What does width times height describe? | The total number of pixel locations. |
| Does the image mode change the number of locations? | No. It changes how much data describes each location. |
| Does RGB make an image three times as wide? | No. It gives each pixel three channels while keeping the same width and height. |

---

## Part 14.4 - Coordinates Access Individual Pixels

A PIL pixel access object lets us inspect and change individual pixels.

```python
img = Image.new("RGB", (10, 10), color=(255, 255, 255))
pixels = img.load()

pixels[3, 4] = (255, 0, 0)

img.save("one_red_pixel.png")
```

| Question | Answer |
| :--- | :--- |
| True or false: the image begins as white. | **True.** Every pixel begins as (255, 255, 255). |
| What does img.load provide? | An object for accessing pixel values by coordinate. |
| What coordinate is changed? | (3, 4). |
| What does PIL use as the first coordinate? | The horizontal coordinate x, or column. |
| What does PIL use as the second coordinate? | The vertical coordinate y, or row. |
| What color is assigned to that location? | Red: (255, 0, 0). |
| What happens to the other pixels? | They remain white. |

### Fundamental Questions About PIL Coordinates

| Question | Answer |
| :--- | :--- |
| What does pixels[x, y] mean? | The pixel at column x and row y. |
| Is this written pixels[y, x]? | No. PIL's direct pixel access uses (x, y). |
| What is the top-left coordinate? | (0, 0). |
| What is the bottom-right coordinate for a 10 x 10 image? | (9, 9). |
| Is (10, 4) valid in a 10 x 10 image? | No. The largest valid x is 9. |

> ### Law of PIL Coordinates
> PIL pixel access always reads `pixels[x, y]` — column first, then row — the same order used everywhere else for a coordinate, never `[y, x]`.

### Visual Check: One Red Pixel

The red pixel may be difficult to see at its original size.

| Question | Answer |
| :--- | :--- |
| Why might the image look entirely white when opened? | One pixel is tiny compared with the whole image. |
| Is the red pixel absent? | No. It is present at coordinate (3, 4). |
| What could make it easier to inspect? | Enlarge the image with nearest-neighbor scaling. |
| What does this example connect to? | A coordinate identifies a location, and a pixel value identifies its state. |

---

## Part 14.5 - A Pixel List Is a Flat Grid

PIL can also receive pixels as one flat list.

```python
width, height = 128, 64
size = width * height

pixels = [0 for _ in range(size)]
pixels[100] = 255

img = Image.new("L", (width, height))
img.putdata(pixels)
img.save("one_white_pixel.png")
```

| Question | Answer |
| :--- | :--- |
| True or false: pixels contains one value for every image location. | **True.** Its length is width * height. |
| What is the length of pixels? | 128 * 64 = 8192. |
| What does pixels[100] = 255 do? | It changes one flat-list value to white. |
| What does putdata do? | It places the flat list into the image as pixel data. |
| Does index 100 visibly tell us its row and column? | No. We need the width to interpret it spatially. |
| What does this resemble from Chapter 9? | A one-dimensional gridworld representation. |

### Fundamental Questions About Flat Pixel Storage

| Question | Answer |
| :--- | :--- |
| What does the flat list preserve? | Every pixel value and its order. |
| What does it hide? | The row boundaries. |
| What information is needed to recover the rows? | The image width. |
| How many values must a grayscale list contain? | Exactly width * height. |
| How many values describe one RGB pixel? | Three values, but the pixel list still contains one tuple per location. |

> ### Law of Flat Pixel Storage
> A flat pixel list is only as meaningful as the width that accompanies it — the same list interpreted with a different width becomes a different image entirely.

---

## Part 14.6 - From an Index Back to a Coordinate

A flat pixel index can be translated into an (x, y) coordinate.

```python
width = 10
index = 25

x = index % width
y = index // width

print(x, y)
```

| Question | Answer |
| :--- | :--- |
| What is index % width? | 5, the column coordinate. |
| What is index // width? | 2, the row coordinate. |
| What coordinate does index 25 represent? | (5, 2). |
| Why is the coordinate not (2, 5)? | The remainder gives the horizontal position; integer division gives the row. |
| What does this repeat from Chapter 9? | The inverse mapping between flat storage and spatial coordinates. |
| Why is this useful for PIL? | A generated or changed pixel can be explained spatially. |

### Fundamental Questions About Width

| Question | Answer |
| :--- | :--- |
| Does an index have a fixed coordinate by itself? | No. Its coordinate depends on the width. |
| Can the same flat list be interpreted as different rectangles? | Yes, if the dimensions change. |
| Why must a saved image include its dimensions? | Dimensions are part of the meaning of the flat pixel sequence. |
| What is the central storage relationship? | index = y * width + x. |

---

## Part 14.7 - Scaling Makes Small Images Visible

A tiny generated image may be correct but difficult to inspect.

```python
scale = 20
new_size = (width * scale, height * scale)

large_img = img.resize(new_size, Image.NEAREST)
large_img.save("scaled.png")
```

| Question | Answer |
| :--- | :--- |
| True or false: scaling changes the number of pixels in the output image. | **True.** The output has more pixel locations. |
| What does scale control? | How much larger the image becomes. |
| What does Image.NEAREST do? | It enlarges pixels without blending neighboring colors. |
| Why is nearest-neighbor scaling useful here? | Individual generated pixels remain visible as sharp blocks. |
| Does resizing change the original image object? | No. resize returns a new image. |
| What is the purpose of the larger image? | Inspection, not the generation of new detail. |

### Visual Check: Nearest-Neighbor Scaling

| Question | Answer |
| :--- | :--- |
| Does nearest-neighbor scaling invent smooth transitions? | No. It repeats the existing pixel values. |
| What might another resampling method do? | Blend neighboring pixels and make a small image look smoother. |
| Is smoother always more accurate for inspecting generated grids? | No. Smoothing can hide the exact cell structure. |
| What does a large nearest-neighbor image show? | The original low-resolution data at a visible scale. |

---

## Part 14.8 - A Nested Loop Fills an Image

The same nested loop used for gridworlds can produce every image pixel. Chapter 4 raised a concern about growing a list one `append` at a time when the final length is already known. An image is exactly that case: `width * height` is known before a single pixel is generated, so the pixel list can be pre-sized and filled by index instead.

```python
width, height = 200, 10

index_of = {
    (x, y): y * width + x
    for y in range(height)
    for x in range(width)
}

pixels = [0] * (width * height)

for y in range(height):
    for x in range(width):
        pixels[index_of[(x, y)]] = x

img = Image.new("L", (width, height))
img.putdata(pixels)
img.save("horizontal_gradient.png")
```

| Question | Answer |
| :--- | :--- |
| True or false: the loop visits width * height coordinates. | **True.** It visits every coordinate. |
| What does index_of store? | A lookup dictionary mapping every `(x, y)` coordinate to its position in the flat pixel list. |
| Where have we seen `y * width + x` before? | Chapter 9. It is the same coordinate-to-index formula used for a flat gridworld. |
| Why compute the formula once into a dictionary instead of writing `y * width + x` inline in the loop? | The lookup is built once and reused; every pixel afterward is a dictionary lookup instead of a repeated calculation. |
| What does `pixels[index_of[(x, y)]] = x` do that `pixels.append(x)` does not? | It writes directly into an already-existing slot, the way Chapter 4's pre-sized list did. |
| What does the outer loop control? | The row, or y coordinate. |
| What does the inner loop control? | The column, or x coordinate. |
| What value fills each location? | The current x coordinate. |
| What happens across one row? | Values increase from left to right. |
| What happens from one row to the next? | The same horizontal pattern repeats. |

### Fundamental Questions About the Image Loop

| Question | Answer |
| :--- | :--- |
| What does the loop visit? | Every pixel coordinate in row-major order. |
| What does the generated value represent? | The pixel state at that coordinate. |
| What makes this a horizontal gradient? | Brightness depends on x. |
| How would we make a vertical gradient? | Store y instead of x at each position. |
| What would a diagonal rule use? | A value derived from both x and y. |
| Is a lookup dictionary required for a complete rectangle? | No. `y * width + x` could be written directly in the loop instead. It becomes worth precomputing once it will be reused by many pixel rules, as the next chapter's generators do. |

> ### Law of Precomputed Position
> When the same coordinate-to-index relationship will be looked up many times, compute it once into a dictionary rather than recalculating it on every pixel.

### Visual Check: Horizontal and Vertical Gradients

| Question | Answer |
| :--- | :--- |
| What should the horizontal gradient show? | A change in brightness from left to right. |
| What should the vertical version show? | A change in brightness from top to bottom. |
| Why is this more than a color trick? | It shows a coordinate becoming a pixel value. |
| What is the bridge to Chapter 15? | A procedural rule can now generate the entire image. |

---

## Part 14.9 - RGB Pixels Are Tuples

The same loop can generate a tuple for each pixel, filled the same pre-sized way.

```python
width, height = 10, 10

index_of = {
    (x, y): y * width + x
    for y in range(height)
    for x in range(width)
}

pixels = [(0, 0, 0)] * (width * height)

for y in range(height):
    for x in range(width):
        pixels[index_of[(x, y)]] = (128, x * 10, 210)

img = Image.new("RGB", (width, height))
img.putdata(pixels)
img.save("rgb_pattern.png")
```

| Question | Answer |
| :--- | :--- |
| True or false: every generated pixel is an RGB tuple. | **True.** Each value has red, green, and blue components. |
| Does the placeholder `(0, 0, 0)` used to pre-size pixels ever reach the image? | No. Every position is overwritten once, since the loop visits every coordinate. |
| What remains constant? | Red stays 128; blue stays 210. |
| What changes? | Green depends on x. |
| What happens as x increases? | The green channel becomes stronger. |
| What kind of image does this create? | A color pattern whose color changes across columns. |
| What does this add to the grayscale example? | Each coordinate now produces a three-channel state instead of one brightness value. |

### Fundamental Questions About Pixel Types

| Question | Answer |
| :--- | :--- |
| What does mode "L" expect? | One valid grayscale value per pixel. |
| What does mode "RGB" expect? | One three-value color tuple per pixel. |
| Can a grayscale list be placed directly into an RGB image? | No. The representations do not match. |
| Can an RGB tuple be placed directly into a grayscale image? | No. The grayscale image expects one value. |
| Why must the mode and pixel data agree? | The image needs to know how to interpret each pixel value. |

> ### Law of Pixel Types
> An image's mode sets the shape every pixel must have — one integer for `"L"`, one three-value tuple for `"RGB"` — and mismatched data is a representation error, not a visual one.

---

## Part 14.10 - Anticipating Errors

PIL makes the representation visible, which also makes mistakes visible.

| Question | Answer |
| :--- | :--- |
| True or false: a grayscale value of 300 is valid in the ordinary "L" range. | **False.** The usual range ends at 255. |
| Is 10.5 an ordinary grayscale pixel value? | No. Grayscale pixels are represented by integer brightness values. |
| What happens if a grayscale image receives RGB tuples? | The data does not match the image mode and PIL raises an error. |
| What happens if an RGB image receives plain integers? | The data does not match the expected three-channel pixel representation. |
| What happens if a pixel list has too few values? | The list does not describe every image location. |
| What happens if it has too many values? | The list does not match the image's expected size. |
| Why are these useful errors? | They reveal the rules of the representation. |

### Fundamental Questions About Representation Errors

| Question | Answer |
| :--- | :--- |
| What kind of error is a wrong pixel range? | A value error: the state is outside the allowed range. |
| What kind of error is a wrong pixel shape? | A type or representation error: the image expects a different kind of value. |
| What kind of error is a wrong list length? | A size or cardinality error: the data does not cover the image correctly. |
| What should we ask when an image operation fails? | What mode, value range, pixel shape, and dimensions does the operation require? |
| Why anticipate errors before running code? | The representation rules can often predict whether the operation is valid. |

---

## Part 14.11 - Choosing Between Direct Access and putdata

PIL provides more than one way to change pixels.

| Question | Answer |
| :--- | :--- |
| True or false: direct pixel access is useful for changing one or a few pixels. | **True.** A coordinate can be changed directly. |
| True or false: putdata is useful for supplying a large collection of pixel values. | **True.** It receives the full flat pixel sequence. |
| Which approach is clearer for drawing one red dot? | Direct access with pixels[x, y]. |
| Which approach is natural for generating a gradient? | Build a pixel list and use putdata. |
| Which approach better matches a whole-image procedural generator? | putdata, because the procedure generates the complete pixel sequence. |
| Is one method always better? | No. The useful method depends on whether the operation is local or whole-image. |

### Fundamental Questions About Local and Global Image Operations

| Question | Answer |
| :--- | :--- |
| What is a local image operation? | A change to one or a small number of coordinates. |
| What is a global image operation? | A process that calculates or replaces the image's complete pixel collection. |
| What does direct access emphasize? | Location. |
| What does putdata emphasize? | The complete flat representation. |
| How does this echo Chapter 12? | One function can perform a local update, while another can compose a whole-image transformation. |

---

## Part 14.12 - PIL as a New Representation

PIL does not replace the ideas from earlier chapters.

It gives those ideas a visible result.

| Question | Answer |
| :--- | :--- |
| What does a coordinate identify? | A pixel location. |
| What does a pixel value identify? | The state or color at that location. |
| What does a flat list provide? | One-dimensional storage for the rectangular image. |
| What does width provide? | The rule for interpreting flat storage as rows. |
| What does the nested loop provide? | A systematic visit to every coordinate. |
| What does putdata provide? | A way to place generated pixel values into an image. |
| What does save provide? | Persistence for the visual result. |
| What will Chapter 15 add? | Procedures that deliberately generate image values and patterns. |

### Fundamental Questions About the Image Representation

| Question | Answer |
| :--- | :--- |
| Is an image separate from data? | No. An image is one way data is organized and displayed. |
| Is PIL separate from procedural generation? | No. PIL provides the container; procedural rules can provide the pixels. |
| What is the important bridge? | A coordinate-based rule can generate a flat pixel list, and PIL can display it. |
| What is the next question? | What kinds of images can a procedure create? |

---

## Part 14.13 - Predict the Result

Do not run these immediately. Predict the final value and the data type.

| Question | Answer |
| :--- | :--- |
| What is img.mode?<br><br>img = Image.new("L", (20, 10), color=0)<br>img.mode | "L", a string. |
| What is img.size?<br><br>img = Image.new("RGB", (64, 128), color=(0, 255, 0))<br>img.size | (64, 128), a tuple. |
| What is the total number of pixels?<br><br>width, height = 4, 3<br>size = width * height | 12, an integer. |
| What is the pixel value?<br><br>img = Image.new("L", (5, 5), color=0)<br>pixels = img.load()<br>pixels[2, 3] = 255<br>pixels[2, 3] | 255, an integer. |
| What is the coordinate?<br><br>width = 4<br>index = 9<br>x = index % width<br>y = index // width | (1, 2), a coordinate tuple. |
| What is pixels?<br><br>width, height = 3, 2<br>pixels = [0 for _ in range(width * height)] | A list of six zeroes. |
| What does the horizontal rule produce?<br><br>pixels = [x for y in range(2) for x in range(4)] | [0, 1, 2, 3, 0, 1, 2, 3], a list. |
| What is the RGB pixel value?<br><br>pixel = (128, 20, 210)<br>pixel[1] | 20, an integer. |
| What must be true before putdata can fill the image? | The pixel sequence must contain exactly width * height values of the correct type for the image mode. |

---

## Chapter Summary

| Question | Answer |
| :--- | :--- |
| What is a grayscale pixel? | One integer brightness value. |
| What is an RGB pixel? | A tuple containing red, green, and blue values. |
| What do width and height describe? | The rectangular dimensions of the image. |
| How many pixel locations does an image contain? | width times height. |
| What does pixels[x, y] provide? | Direct access to one pixel at one coordinate. |
| What does a flat pixel list provide? | One-dimensional storage for all image locations. |
| What does putdata do? | Places a complete flat pixel sequence into an image. |
| What does Image.NEAREST help us see? | Enlarged pixels without blended boundaries. |
| Why do pixel mode and data type matter? | PIL must know how to interpret each stored value. |
| What is the central idea? | An image is a rectangular grid of data, and PIL lets Python create, inspect, transform, and save that data visually. |

---

**==> The image is not the mystery. The pixels are the picture. <==**
