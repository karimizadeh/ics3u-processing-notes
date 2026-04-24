# Two Dimensional Lists

A list keeps track of multiple pieces of information in linear order or in a single dimension.
However, as you have experienced by completing the tic-tac-toe assignment, data associated with certain systems live in two dimensions.
A two-dimensional list is nothing more than a list of lists (a three-dimensional list is a list of lists of lists, so on and so forth for higher dimension lists).

In the case of a list, our old-fashioned 1D list look like this:

```python
myList = [0, 1, 2, 3]
```

2D lists look like this:

```python
myList = [[0,1,2,3], [3,2,1,0], [3,5,6,1], [3,8,3,4]]
```

It might be easier to think of the 2D list as a matrix.
A matrix can be thought of as a grid of numbers, arranged in rows and columns:

```python
myList = [
    [0, 1, 2, 3],
    [3, 2, 1, 0],
    [3, 5, 6, 1],
    [3, 8, 3, 4]
]
```

To iterate through a 1D list, we use a single loop:

```python
myList = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
for index in range(len(myList)):
    myList[index] = 0 # Set element at "index" to 0. 

print(myList)
```

For 2D lists, we need to reference every element so we must use two nested loops:

```python
myList= [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8]
]

# Two nested loops allow us to visit every spot in a 2D list.   
# For every column i, visit every row j.
for i in range(len(myList)):
    for j in range(len(myList[i])):
        myList[i][j] = 0

print(myList)
```

You have experienced using 2D lists in your tic-tac-toe assignment.
However, you can use 2D lists in other ways instead of just as a grid.

## Example - 2D List

```python
def setup():
    size(200, 200)
    nRows = height
    nCols = width
    myList = make2dList(nRows, nCols)
    drawPoints(myList)

def make2dList(nRows, nCols):
    newList = []
    for row in xrange(nRows):
        # give each new row an empty list
        newList.append([])
        for col in xrange(nCols):
            # Make every column in every row a random int from 0 to 255
            newList[row].append(int(random(255)))

    return newList

def drawPoints(pointList):
    for y in xrange(len(pointList)):
        for x in xrange(len(pointList[0])):
            stroke(pointList[y][x])
            rect(x, y, 10, 10)
```

## Example - Advanced

```python
# Number of columns and rows in the grid
nCols = 10
nRows = 10

def setup():
    global nCols, nRows, grid
    size(200, 200)
    grid = makeGrid()
    for i in xrange(nCols):
        for j in xrange(nRows):
            # Initialize each object
            grid[i][j] = Cell(i * 20, j * 20, 20, 20, i + j)

def draw():
    global nCols, nRows, grid
    background(0)
    # The counter variables i and j are also the column and row numbers and 
    # are used as arguments to the constructor for each object in the grid.  
    for i in xrange(nCols):
        for j in xrange(nRows):
            # Oscillate and display each object
            grid[i][j].oscillate()
            grid[i][j].display()

# Creates a 2D List of 0's, nCols x nRows large
def makeGrid():
    global nCols, nRows
    grid = []
    for i in xrange(nCols):
        # Create an empty list for each row
        grid.append([])
        for j in xrange(nRows):
            # Pad each column in each row with a 0
            grid[i].append(0)
    return grid

# A Cell object 
# NOTE: We haven't learned about Classes/Objects yet and you wont be expected to know/understand the code below
class Cell():
    # A cell object knows about its location in the grid 
    # it also knows of its size with the variables x,y,w,h.
    def __init__(self, tempX, tempY, tempW, tempH, tempAngle):
        self.x = tempX
        self.y = tempY
        self.w = tempW
        self.h = tempH
        self.angle = tempAngle

    # Oscillation means increase angle
    def oscillate(self):
        self.angle += 0.02;

    def display(self):
        stroke(255)
        # Color calculated using sine wave
        fill(127 + 127 * sin(self.angle))
        rect(self.x, self.y, self.w, self.h)
```
