# Processing

Processing is a simple programming environment that was created to make it easier to develop visually oriented applications with an emphasis on animation, as opposed to the command line interface programs you have written thus far.

Python Mode for Processing is an extension to Processing.
It allows you to write Processing programs in the Python.

## Drawing a Line

The processing equivalent of a "Hello World" program is simply to draw a line.
In the text editor, type the following:

```python
line(15, 25, 70, 90)
```

This line of code is drawing a black line for coordinate (15, 25) to (79, 90).
Coordinates (0, 0) are the upper left-hand corner of the display window.
Click the "play" button to run.

Now, lets change the size of the display window and set the background colour:

```python
size(400, 400) # setting the window size to 400 x 400 pixels
background(192, 64, 0) # setting the background to an orange-red
stroke(255) # setting the colour of the line to 255
line(150, 25, 270, 350) # drawing of the line
```

Other variations of the `stroke()` with alternate results:

```python
stroke(255) # sets the stroke color to white
stroke(255, 255, 255) # identical to the line above
stroke(255, 128, 0) # bright orange (red 255, green 128, blue 0)
stroke("#FF8000") # bright orange as a hexadecimal colour
stroke(255, 128, 0, 128) # bright orange with 50% transparency
```

The stroke function affects all geometry drawn to the screen until the next stroke function is called.

## Processing Programs

A program written as a list of statements (like the above code) is called a static sketch
In a static sketch, a series of functions are used to perform tasks.
Interactive programs are drawn as a series of frames, which you can create by adding functions titled `setup()` and `draw()`.

The `setup()` function runs once and the `draw()` function runs repeatedly every frame.
`setup()` is used for any initialization, such as setting the screen size, making the background orange, setting the stroke colour to white, etc.
`draw()` is used to handle animation and drawing frames.
`size()` MUST ALWAYS be the first line inside `setup()`.

```python
def setup():
    size(400, 400)
    stroke(255)
    background(192, 64, 0)

def draw():
    line(150, 25, mouseX, mouseY)
```

Because `background()` is used once in the `setup()`, the screen will fill with lines as the mouse is moved
If we move `background()` to draw, it will draw a single line that follows the mouse.

```python
def setup():
    size(400, 400)
    stroke(255)

def draw():
    background(192, 64, 0)
    line(150, 25, mouseX, mouseY)
```

## Ellipses

In the text editor, type the following:

```python
ellipse(50, 50, 80, 80)
```

This line of code is drawing an ellipse, with the centre 50 pixels over from the left and 50 pixels down from the top, with a width and height of 80 pixels.

Delete the line above and enter the following code:

```python
def setup():
    size(480, 120)

def draw():
    if mousePressed:
        fill(0)
    else:
        fill(255)
        ellipse(mouseX, mouseY, 80, 80)
```

Similar to the line program, this one follows your cursor and draws ellipses as it goes. If you press down, it fills the ellipse with the colour black, otherwise, it is white.

Going forward, this is how I want to see you code. You must have a `setup()` function that sets the window size, colour, etc. Then a `draw()` function that handles drawing the objects.
You may have additional functions to help, but those functions should be called in `draw()`.

## Creating an Image From Your Work

You can save an image of your work by adding `saveFrame()` to the end of `draw()`.
Be careful, as a new image will be saved each time `draw()` runs, which can create a lot of images very quickly.
To find your images, click the "Sketch" menu and then select "Show Sketch Folder."

```python
saveFrame()
saveFrame("output-#####.png") # You can also specify the name and filetype
```

## Loading and Display Data

To add a file to the data folder of a Processing sketch, click "Sketch" on the menu bar and select "Add File."

```python
def setup():
    global img
    img = loadImage("./download.png")

def draw():
    background(0)
    image(img, 0, 0)
    stroke(255)
    line(150, 25, 70, 90)
```

The `img` variable has to be set to `global` in `setup()` so you have access to it in `draw()` and any other function that might use it.
Without setting it to `global`, the `img` variable does not exist outside the `setup()` function.

## Comments

In Python, we were only able to make comments one line at a time.
In Processing, we can have multiline comments using the following syntax:

```python
"""
comment
comment
"""
# OR
'''
comment
comment
'''
```

## Colours

When it comes to colours in the digital world, precision is required.
Colour is defined as a range of numbers from 0 to 255.
Black is 0 and white is 255.
Every other other in-between is a share of grey ranging from black to white.
Colour needs to be stored in the computer's memory.
This memory is just one long sequence of 0's and 1's.
Each one of these switches is a bit, eight of them together is a byte.
By having eight bits (1 byte), we have 256 possibilities (0 to 255). We use 8 bit colour for grayscale and 24 bit for full colour (eight bits of red, green, and blue).
Earlier, we used `stroke()` to fill in the colour of the 'line' to white.
We need to add `stroke()` before something is drawn to set the colour of any given shape.

`stroke()` is setting the outline of the "brush."
`fill()` is setting the interior of the shape drawn.

```python
def setup():
    background(255) # Setting the background to white

def draw():
    stroke(0) # Setting the outline (stroke) to black
    fill(150) # Setting the interior of a shape (fill) to grey
    rect(50,50,75,100) # Drawing the rectangle
```

`stroke()` or `fill()` can be eliminated with the functions `noStroke()` and `noFill()` because using `stroke(0)` or `fill(0)` just sets it to black.
If we draw two shapes, Processing will always use the most recently specified stroke and fill, reading the code from top to bottom.

```python
def setup():
background(150)

def draw():
stroke(0)
line(0,0, 100, 100)
stroke(255)
noFill()
rect(25, 25, 50, 50)
```

## RGB Colours

Similar to how you discovered colours are made through finger painting the three primary colours, didn't colours are constructed almost the same way.
Instead of mixing them with your fingers, we are mixing a range of 0 to 255 of Red, Green, and Blue to create any other colours.

```python
def setup():
    background(255)
    noStroke()

def draw():
    # Bright red
    fill(255, 0, 0)
    ellipse(20, 20, 16, 16)

    # Dark red
    fill(127, 0, 0)
    ellipse(40, 20, 16, 16)

    # Pink (pale red)
    fill(255, 200, 200)
    ellipse(60, 20, 16, 16)
```

In addition to setting the colours, we can set the transparency of our colours.
Transparency also ranges from 0 to 255, 0 being completely transparent and 255 being completely opaque.

```python
def setup():
    size(200, 200)
    background(0)
    noStroke()

def draw():
    # No fourth argument means 100% opacity.
    fill(0, 0, 255)
    rect(0, 0, 100, 200)

    # 255 means 100% opacity.
    fill(255, 0, 0, 255)
    rect(0, 0, 200, 40)

    # 75% opacity.
    fill(255, 0, 0, 191)
    rect(0, 50, 200, 40)

    # 55% opacity.
    fill(255, 0, 0, 127)
    rect(0, 100, 200, 40)

    # 25% opacity.
    fill(255, 0, 0, 63)
    rect(0, 150, 200, 40)
```

Now, we know that colours arent just black, white, red, green, blue, and the range of shades in-between each individual colour.
We can also mix colours to have colours like purple or yellow.
To create these colours, we use `colorMode()`.

```python
# max1 is the range in red, max2 is the range in green, max3 is the range in blue
colorMode(RGB, max1, max2, max3, transparency)
# we specify RGB so it knows we mean Red, Green, Blue
colorMode(RGB, 100, 500, 10, 255)
```

### Example

```python
noStroke()
colorMode(RGB, 100)
for i in range(100):
    for j in range(100):
        stroke(i, j, 0)
        point(i, j)
```

We can also specify colours in HSB: hue, saturation, and brightness.

- Hue - the color type, ranges from 0 to 255
- Saturation - the vibrancy of the colour from 0 to 255
- Brightness - the brightness of the colour from 0 to 255

```python
colorMode(HSB, 100, 500, 10, 255)
```

### Example

```python
noStroke()
colorMode(HSB, 100)
for i in range(100):
    for j in range(100):
        stroke(i, j, 100)
        point(i, j)
```
