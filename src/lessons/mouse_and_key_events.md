# Mouse and Key Events

Continuing on from last lesson...

## More Mouse Data Examples

Use `mouseX` and `mouseY` variables with an if-statement to allow the cursor to select regions of the screen.

### Example 1

```python
def setup():
    size(100, 100)
    noStroke()
    fill(0)

def draw():
    background(204)
    if mouseX < 50:
        rect(0, 0, 50, 100) # Left
    else:
        rect(50, 0, 50, 100) # Right
```

### Example 2

```python
def setup():
    size(100, 100)
    noStroke()
    fill(0)

def draw():
    background(204)
    if mouseX < 33:
        rect(0, 0, 33, 100) # Left
    elif mouseX < 66:
        rect(33, 0, 33, 100) # Middle
    else:
        rect(66, 0, 33, 100) # Right
```

If we add a logical operator to the if statement, we can modify the above code to select a rectangular region of the screen.
It is testing each edge of the rectangle by checking if the cursor is to the right of the leftmost edge,
if the cursor is to the left of the rightmost edge, if the cursor is beyond the topmost edge, and if the cursor above the bottommost edge.

```python
def setup():
    size(100, 100)
    noStroke()
    fill(0)

def draw():
    background(204)
    if ((mouseX > 40) and (mouseX < 80) and
      (mouseY > 20) and (mouseY < 80)):
        fill(255)
    else:
        fill(0)
    rect(40, 20, 40, 60)
```

The code below asks a similar set of questions and combines them with `elif` and `else` statements to check which defined area contains the cursor:

```python
def setup():
    size(100, 100)
    noStroke()
    fill(0)

def draw():
    background(204)
    if ((mouseX <= 50) and (mouseY <= 50)):
        rect(0, 0, 50, 50)   # Upper-left
    elif ((mouseX <= 50) and (mouseY > 50)):
        rect(0, 50, 50, 50)   # Lower-left
    elif ((mouseX > 50) and (mouseY <= 50)):
        rect(50, 0, 50, 50)   # Upper-right
    else:
        rect(50, 50, 50, 50)   # Lower-right
```

## Keyboard Data

Processing can also register if a key on the keyboard has been pressed.
Similarly to mouse data variables, there is a built-in variable called `keyPressed`.
`keyPressed` is `True` if a key is pressed (and held) and `False` if not.

### Example 1

```python
def setup():
    size(100, 100)
    strokeWeight(4)

def draw():
    background(204)
    if keyPressed: # If the key is pressed,
        line(20, 20, 80, 80) # draw a line
    else: # Otherwise,
        rect(40, 40, 20, 20) # draw a rectangle
```

### Example 2

```python
x = 20

def setup():
    size(100, 100)
    strokeWeight(4)

def draw():
    global x
    background(204)
    if keyPressed: # If the key is pressed
        x += 1 # add 1 to x 
    line(x, 20, x - 60, 80) # the line will move with each keyPress
```

The built-in `key` variable will storage a single alphanumeric character.
Specifically, it holds the most recently pressed key.
You can display this on the screen using the `text()` function:

```python
def setup():
    size(100, 100)
    textSize(60)

def draw():
    background(0)
    text(key, 20, 75) # Draw at coordinate (20, 75)
```

You can use the `text()` function to write any arbitrary text to the screen:

```python
def setup():
    size(100, 100)
    textSize(60)

def draw():
    background(0)
    text(":)", 20, 75) # Draw at coordinate (20, 75)
```

You can use the `key` variable to determine whether a specific key is pressed.

NOTE: You have to use single quotes for the letters as it signifies the data is of the data type `char` (char short for character).
Using double quotes will cause an error because the double quotes signify the data is of the type `string`.
You cannot compare a `string` with a `char` in Processing.

```python
def setup():
    size(100, 100)
    strokeWeight(4)

def draw():
    background(204)
    # If the 'A' key is pressed draw a line
    if ((keyPressed) and ((key == 'a') or (key == 'A'))):
        line(50, 25, 50, 75)
    else: # Otherwise, draw an ellipse
        ellipse(50, 50, 50, 50)
```

## ASCII Ordering

As you guys should remember, each character has a numeric value as defined by the ASCII table.
The value of the key variable can be used like any other number to control visual attributes such as the position and colour of shape elements.
In order to extract this value in Python Mode, we use the `ord()` function.
This function will convert any ASCII character into its numerical ASCII code.

### Example 1

```python
def setup():
    size(100, 100)
    stroke(0)

def draw():
    if keyPressed:
        x = ord(key) - 32
        line(x, 0, x, height)
```

### Example 2

```python
angle = 0

def setup():
    size(100, 100)
    fill(0)


def draw():
    global angle
    background(204)
    if keyPressed:
        if ((ord(key) >= 32) and (ord(key) <= 126)):
            # If the key is alphanumeric, use its value as an angle
            angle = (ord(key) - 32) * 3

    arc(50, 50, 66, 66, 0, radians(angle))
```

## Coded Keys

In addition to reading key values for numbers, letters, and symbols, Processing can also read the values from other keys such as arrow keys.
The variable `keyCode` stores `ALT`, `CONTROL`, `SHIFT`, `UP`, `DOWN`, `LEFT`, and `RIGHT` as constants.
Before determining which coded key is pressed, it is necessary to first check to see if `key` is coded:

```python
y = 35

def setup():
  size(100, 100)

def draw():
    global y
    background(204)
    line(10, 50, 90, 50)
    if key == CODED: # True if the key is coded and False otherwise
        if keyCode == UP:
            y = 20
        elif keyCode == DOWN:
            y = 50
    else:
        y = 35
    rect(25, y, 50, 30)
```

NOTE: `BACKSPACE`, `TAG`, `ENTER`, `RETURN`, `ESC`, and `DELETE` will not be identified as coded keys.

## Events

A category of functions called events alter the normal flow of a program when an action such as a key press or mouse movement takes place.
Key presses and mouse movements are stored until the end of `draw()`, where they can take actions that wont disturb drawing that is currently in progress.
The code inside an event function is run once each time the corresponding event occurs.

### Mouse Events

The following are the mouse event functions.

Mouse Event Function | Description
-|-
`mousePressed()` | code inside this block is run one time when a mouse button is pressed
`mouseReleased()` | code inside this block is run one time when a mouse button is released
`mouseMoved()` | code inside this block is run one time when a mouse button is moved
`mouseDragged()` | code inside this block is run one time when a mouse button is moved while the mouse button is pressed

The `mousePressed()` function works differently than the `mousePressed` variable.
The value of the `mousePressed` variable is `True` until the mouse button is released.
It can be used within `draw()` to have a line of code run while the mouse is pressed.
In contrast, the `mousePressed()` function only runs once when a button is pressed.
This makes it useful when a mouse click is used to trigger an action, such as clearing the screen.

#### Example 1

```python
gray = 0

def setup():
    size(100, 100)

def draw():
    global gray
    background(gray)

def mousePressed():
    global gray
    gray += 20
```

#### Example2

```python
gray = 0

def setup():
    size(100, 100)

def draw():
    global gray
    background(gray)

def mouseReleased():
    global gray
    gray += 20
```

The code inside `mouseMoved()` and `mouseDragged()` event functions are run when there is a change in the mouse position.
The code in the `mouseMoved()` block is run at the end of each frame when the mouse moves and no button is pressed.
The code in the `mouseDragged()` block is run when the mouse moves and a button is pressed.

```python
# Initialize moveX, moveY, dragX, dragY off screen
moveX = moveY = dragX = dragY = -20

def setup():
    size(100, 100)
    noStroke()

def draw():
    global moveX, moveY, dragX, dragY
    background(204)
    fill(0)
    ellipse(dragX, dragY, 33, 33) # Black circle
    fill(153)
    ellipse(moveX, moveY, 33, 33) # Gray circle

def mouseMoved(): # Move gray circle
    global moveX, moveY
    moveX = mouseX
    moveY = mouseY

def mouseDragged(): # Move black circle
    global dragX, dragY
    dragX = mouseX
    dragY = mouseY
```

### Key Events

There are two keyboard event functions:

Key Event Function | Description
-|-
`keyPressed()` | code inside this block is run one time when any key is pressed
`keyReleased()` | code inside this block is run one time when any key is released

#### Example 1

```python
drawT = False

def setup():
  size(100, 100)
  noStroke()

def draw():
    global drawT
    background(204)
    if drawT:
        rect(20, 20, 60, 20)
        rect(39, 40, 22, 45)
  

def keyPressed():
    global drawT
    if ((key == 'T') or (key == 't')):
        drawT = True

def keyReleased():
    global drawT
    drawT = False
```

## Event Flow

Programs written with `draw()` display frames to the screen 60 frames each second.
The `frameRate()` function is used to set a limit on the number of frames that will display each second.
The `noLoop()` function can be used to stop `draw()` from looping.
The additional functions `loop()` and `redraw()` provide more options when used in combination with the mouse and keyboard event functions.
If a program has been paused with the `noLoop()` function, running the `loop()` function resumes its action.
Because event functions are the only elements that continue to run when a program is paused with `noLoop()`, `loop()` can be used within these functions to continue running the code in `draw()`.

```python
frame = 0

def setup():
    size(100, 100)

def draw():
    global frame
    if frame > 120: # If 120 frames since the mouse
        noLoop() # was pressed, stop the program
        background(0) # and turn the background black.
    else: # Otherwise, set the background
        background(204) # to light gray and draw lines
        line(mouseX, 0, mouseX, 100) # at the mouse position
        line(0, mouseY, 100, mouseY)
        frame += 1

def mousePressed():
    global frame
    loop()
    frame = 0
```

The `redraw()` function runs code in `draw()` one time and then halts the execution.
It is helpful when the display doesn't need to be updated continuously.
The following example runs the code in `draw()` once each time a mouse button is pressed.

```python
def setup():
    size(100, 100)
    noLoop()

def draw():
    background(204)
    line(mouseX, 0, mouseX, 100)
    line(0, mouseY, 100, mouseY)

def mousePressed():
    redraw() # Run the code in draw one time
```

## Cursor

The cursor can be hidden with the `noCursor()` function, and can be set to appear as a different icon/image with the `cursor()` function.
When `noCursor()` is run, the cursor icon disappears as it moves into the display window.

### Example 1

```python
def setup():
    size(100, 100)
    strokeWeight(7)
    noCursor()

def draw():
    background(204)
    ellipse(mouseX, mouseY, 10, 10)
```

### Example 2

```python
def setup():
    size(100, 100)
    noCursor()

def draw():
    background(204)
    if mousePressed:
        cursor()
```

Add a parameter to the cursor() function to change it to another icon or image.
Either load and use a custom image or use one of the built-in constants: `ARROW`, `CROSS`, `HAND`, `MOVE`, `TEXT`, or `WAIT`.
These cursor icons are part of the operating system and may appear different on different machines.

```python
def setup():
    size(100, 100)

def draw():
    background(204)
    if mousePressed:
        cursor(HAND) # Draw cursor as hand
    else:
        cursor(CROSS)
    line(mouseX, 0, mouseX, height)
    line(0, mouseY, height, mouseY)
```
