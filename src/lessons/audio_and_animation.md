# Audio and Animation

## Audio

Processing uses an audio library called minim.
To install minim, navigate in the toolbar to Sketch > Import Library > Add Library, and in the search bar type "minim", then select and install it.

Just like other Python libraries such as `random`, you must tell your program that you actually want to use the installed library.
However, unlike regular Python libraries, Processing libraries are included via the `add_library()` function as opposed to the `import` keyword.

### Minim

> "I couldn't really find a nice way to make this explanation a non comment-based code block example, sorry"
>
> \- <cite>Timbits<cite/>

```python
add_library('minim')

def setup():
    global intro
    # this initializes the minim Object and assigns it to a variable which we can
    # use later
	minim = Minim(this)

    # Minim is able to load wav files, au files, aif files, snd files, and mp3 files.
    # If you don't have the file, you can load sounds by specifying a URL
	intro = minim.loadFile("Intro.wav")
	sound = minim.loadFile("game.mp3")

	intro.play()

    # delay times are specified in milliseconds.
    # 3000 will stop for 3 seconds
	delay(3000)

	intro.rewind() # rewinds the sound file
	sound.loop() # plays the sound file on a loop

def draw():
    global intro
    # To check if the sound file loaded is playing, we can use the isPlaying() function
	if intro.isPlaying():
		text("music is playing", 10, 60)

    # To check how much of the sound file has currently played,
    # we can use the position() function
	print(intro.position())

    # To check the length of the sound file loaded, we can use the length() function
	print(intro.length())

# We can use the above methods in combination with the keyPressed() function to
# interact with the audio player
def keyPressed():
    # if the sound file is playing, it will pause when any key is pressed
    if intro.isPlaying():
        intro.pause()
    # if the sound file is NOT playing, it will play when any key is pressed
    else:
		intro.play()
```

You can specify specific keys to be pressed to pause, play, and rewind using knowledge we learned in the previous lesson.

#### Audio Sample

You can load audio samples using `minim.loadSample()`.
This is not to be confused with `minim.loadFile()`, and can also load sounds via URL.
Sampling is a special kind of file playback that allows the sound to be repeatedly triggered.
This is achieved by keeping the sample in an internal buffer within memory.
Because of this, sampling should be limited to short sounds that are repeatedly used.
The size of the internal buffer may be adjusted via the second parameter of the `loadSample()` function.
The default buffer size is `1024`.

```python
add_library('minim')

kick = None
snare = None

def setup():
    size(512, 200)
    global kick, snare, minim
    minim = Minim(this)
    kick = minim.loadSample("BD.mp3", 512)

def draw():
	global kick, snare, minim

    # if a file doesn't exist, loadSample will return None
    if kick == None:
        print("Didn't get kick!")

    # load SD.wav from the data folder
    snare = minim.loadSample("SD.wav", 512)
    if snare == None:
        print "Didn't get snare!"

def keyPressed():
    if key == 's':
        snare.trigger()
    if key == 'k':
        kick.trigger()
```

### Sound Library

There are also other sound libraries available in Processing, such as the Sound library.

```python
add_library("sound")

def setup():
    global sf
    size(400, 400)
    sf = SoundFile(this, "Intro.wav")

def mouseClicked():
    global sf
    sf.play()
```

## Inputting Text

> "Why is this here"
>
> \- <cite>Timbits<cite/>

```python
input = ""

def setup():
    size(400,400)
   
def draw():
    background(0)
    text("input text:", 25, 190)
    text(input, 80, 190)

def keyPressed():
    global input
    # if the return/enter key is pressed, save the input and clear it
    if key == '\n':
        saved = input;
        input = ""
        print(saved)
    else:
        # otherwise, concatenate the input
        # Each character typed by the user is added to the end of the input variable
        input += key
```

## Animation

Before writing any animation code, consider how motion is perceived.
The brain is fed a snapshot from your retina around ten times each second.
The speed at which objects appear to be moving (or not moving) is determined by the difference between successive snapshots.
So, provided your screen can display a sequence of static images at a rate exceeding ten cycles per second, the viewer will experience the illusion of smooth flowing movement.
This illusion is referred to as Beta movement and occurs at frame rates of around 10-12 images per second – although higher frame rates will appear even smoother.
That said, there’s more to motion perception than frames per second (fps).

All the is required to get animating in Processing are the `setup()` and `draw()` functions.

First, we will start by writing code in `setup()` like we have for every sketch we done so far.

```python
def setup():
    size(500,500)
    background('#004477')
    noFill()
    stroke('#FFFFFF')
    strokeWeight(3)
```

Next, we will print `frameCount` inside `draw()`.

```python
def draw():
    print(frameCount)
```

`frameCount` is a system variable containing the number of frames displayed since starting the sketch.
By default, `draw()` executes at around 60fps, but as the complexity of an animation increases, the frame rate drops.
You can adjust the frame rate by using the `frameRate()` function.
Our `setup()` and `draw()` functions will now look like this:

```python

def setup():
    # draw() will run 2.5 times every second, making each frame
    # 0.4 seconds in duration
    frameRate(2.5)
    size(500, 500)
    background('#004477')
    noFill()
    stroke('#FFFFFF')
    strokeWeight(3)

def draw():
    if frameCount % 2 == 0: # will print every other frame
        # a new print line will appear in console every 800 milliseconds
        print(frameCount)
```

Now, lets add an ellipse to print every second frame and run the code.

```python
def draw():
    if frameCount % 2 == 0:
        print(frameCount)
        ellipse(250, 140, 47, 47)
```

Notice how the ellipse does not blink?
This is because everything in Processing persists after being drawn - so every second frame, another ellipse is drawn on top of the existing one, forming a pile of ellipses.
The background colour was defined in the `setup()` section and is only drawn once, making it the bottommost layer.
To "clear" each frame before drawing the next, simply move background code to the `draw()` section.

```python
def setup():
    frameRate(2.5)
    size(500, 500)
    noFill()
    stroke('#FFFFFF')
    strokeWeight(3)

def draw():
    background('#004477')
    if frameCount % 2 == 0:
        print(frameCount)
        ellipse(250, 140, 47, 47)
```

Now we can see the ellipse is blinking.
Let's add an else statement and another ellipse in a different spot so the ellipses will alternate:

> "I have no idea where `height` even came from and I don't know what it's supposed to be"
>
> \- <cite>Timbits<cite/>

```python
def setup():
    frameRate(2.5)
    size(500, 500)
    noFill()
    stroke('#FFFFFF')
    strokeWeight(3)

def draw():
    background('#004477')
    if frameCount % 2 == 0:
        print(frameCount)
        ellipse(250, 140, 47, 47)
    else:
        ellipse(250, height - 140, 47, 47)
```

This isn't exactly what we want for an animation.
For the animation to look convincing, we want to see ellipses going from the top to the bottom instead of two blinking ellipses.
To accomplish this without having to write out each frame position, we should use a global variable to store and update the circle's `y` value.
Let's make some modifications to our code:

```python
y = 1

def setup():
    size(500, 500)
    noFill()
    stroke('#FFFFFF')
    strokeWeight(3)
    print(y)

def draw():
    global y
    background('#004477')
    y += 1 # will increment by 1 with each new frame that is drawn
    print(y)
    # a new ellipses will be drawn at every frame using y for its new location
    ellipse(height / 2, y, 47, 47)
```

### Moving Background

> "No explanation on why this might ever be the case but I'm not sure if it's necessary?
> I can see this being meant for more 'advanced' students who already understand frame of reference and other such concepts"
>
> \- <cite>Timbits<cite/>

Instead of moving an object on your screen, sometimes it is easier to move the background itself instead.
To do this, we use the `copy()` function.
The `copy()` function copies a region of pixels from the display window to another area of the display window and copies a region of pixels from an image used as the `srcImg` parameter into the display window.
If the source and destination regions aren't the same, it will automatically resize the source pixels to fit the specified target region.

Syntax
: `copy(sx, sy, sw, sh, dx, dy, dw, dh)`
: `copy(src, sx, sy, sw, sh, dx, dy, dw, dh)`

Parameter | Type | Description
-|-|-
`src` | `PImage` | an image variable referring to the source image
`sx` | `int` | X coordinate of the source's upper left corner
`sy` | `int` | Y coordinate of the source's upper left corner
`sw` | `int` | source image width
`sh` | `int` | source image height
`dx` | `int` | X coordinate of the destination's upper left corner
`dy` | `int` | Y coordinate of the destination's upper left corner
`dw` | `int` | destination image width
`dh` | `int` | destination image height

#### Example:

```python
def setup():
    global cornerPointX, cornerPointY, canvasX, canvasY, back, showCar
    global showCarX, showCarY, carSizeX, carSizeY, backHeight, backWidth, chunkSize, chunkIncr, chunkX, chunkY
    size(800, 600)

    backHeight = 247 # background image height
    backWidth = 788  # background image width

    chunkSize = 100 #  source image width; how much of the image we want to see
    chunkIncr = 1   # increment; used to increase the speed of background image movement
    chunkX = 0      # x coordinate of the source image upper left corner
    chunkY = 0      # y coordinate of the source image upper left corner

    showCarX = 100 # x coordinate of car
    showCarY = 425 # y coordinate of car

    carSizeX = 100 # x size of car
    carSizeY = 50  # y size of car

    cornerPointX = 0 # x coordinate of the destination image upper left corner
    cornerPointY = 0 # y coordinate of the destination image upper left corner

    canvasX = 800 # destination image width
    canvasY = 600 # destination image height

    showCar = loadImage("car.png")
    back = loadImage("country road.PNG")

    # Syntax:
    # copy(src,sx,     sy,     sw,        sh,         dx,           dy,           dw,      dh)
    copy(back, chunkX, chunkY, chunkSize, backHeight, cornerPointX, cornerPointY, canvasX, canvasY)

def draw():
    global cornerPointX, cornerPointY, canvasX, canvasY, back, showCar
    global showCarX, showCarY, carSizeX, carSizeY, backHeight, backWidth, chunkSize, chunkIncr, chunkX, chunkY

    # increments the X coordinate of the source image upper left corner
    chunkX += chunkIncr

    # if the X coordinate and size is greater than the background image width
    if (chunkX + chunkSize) >= backWidth:
        chunkX = 0 # reset it to 0

    # Syntax:
    # copy(src,sx,     sy,     sw,        sh,         dx,           dy,           dw,      dh)
    copy(back, chunkX, chunkY, chunkSize, backHeight, cornerPointX, cornerPointY, canvasX, canvasY)
    image(showCar, showCarX, showCarY, carSizeX, carSizeY) # displays car ontop of moving background image
```
