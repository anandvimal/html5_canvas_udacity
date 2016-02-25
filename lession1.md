take a look at this :
www.w3.org/TR/2dcontext/
********************************************************************************
```
<!DOCTYPE HTML>
<html>
<head>
</head>
<body>
  <canvas id="c" width="200" height="200"></canvas>
  <script>
    var c = document.querySelector("#c");
    var ctx = c.getContext("2d");
  </script>
</body>
</html>
```


we declare the canvas with this line:   

```
<canvas id="c" width="200" height="200"></canvas>
```
after this we need to do few more steps in javascript to get the context of canvas.

we grab the selector with this line:     
```
var c = document.querySelector("#c");
```
and then we grab its context by this line: var
```
ctx = c.getContext("2d");
```
we can also get a 3d canvas.

from here we start drawing on canvas.

now on canvas we draw with 2d co ordinates. to right is positive x. and to down is positive y.
which is not exactly like normal math 2d co ordinates.

and (0,0) is on top left corner.

---

now to add a image in a webpage we usually use the image tags
```
<img src="http://notawebsite.com/image.jpg">
```
but in canvas we will do in script in javascript.

```
var image = new Image();

image.onload = function(){
  console.log("Loaded image");
  // do something else
  ctx.drawImage(image, 0, 0, c.width, c.height);
}

image.src = "fry_fixed.jpg";

Notes: drawImage has three signatures for drawing in a canvas object.
void ctx.drawImage(image, dx, dy);
void ctx.drawImage(image, dx, dy, dWidth, dHeight);
void ctx.drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight);
```

useful links:

 https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/drawImage

---

to save the image we made here we do:

```
var savedImage = c.toDataURL();
window.open(savedImage);
image.onload = function(){
  console.log("Loaded image");
  // do something else
  ctx.drawImage(image, 0, 0, c.width, c.height);
  var savedImage = c.toDataURL();
  window.open(savedImage);
}
```

* the saving thing did not worked so far. so leave this part for now.
may be i will come back to this.

solution: use SimpleHTTPServer if you already have Python installed.
Python comes already installed on most Max OSX and linux machines.
In a terminal change to the directory that has your html files and run the following command :
python -m SimpleHTTPServer
after it starts you can navigate to http://0.0.0.0:8000/index.html

********************************************************************************

how we will draw on canvas:
```
<!DOCTYPE HTML>

<html>
<head>
</head>
<body>
  <canvas id="c" width="200" height="200"></canvas>
  <script>
    var c = document.querySelector("#c");
    var ctx = c.getContext("2d");

    ctx.fillRect(100, 100, 100, 100);
    ctx.strokeRect(50, 50, 50, 50);

  </script>
</body>
</html>
```

filled rectanges are filled with colors and stroke rectanges are not.

********************************************************************************
Erase a rectange:

we can use fillRect and set fillStyle to our background color.
fillStyle : used to select color of filling canvas.

so we will select white as fillStyle and then fullRect the whole canvas.

clearRect : is easier and we don't have to select any color.

if we have a canvas c and given some code to draw a rect in a given color you might write something like:


var c = document.querySelector("#c");
var ctx = c.getContext("2d");
ctx.fillStyle = "blue";

// Start at (0,0) and draw a 50px x 50px blue rectangle.
ctx.fillRect(0,0,50,50);

// Start a (0,0) and clear a 25px x 25px rectangle.
ctx.clearRect(0,0,25,25);

if you want to erase the entire canvas you can call clearRect with the dimensions of canvas as follows:

  ctx.clearRect(0,0,c.width,c.height):

A slightly shorter way to clear a full canvas is to change either the height or the width on the canvas:

  c.width = c.width;
  // doing so saves some keystrokes but may not be as readable by others.

Use clearRect when to erase a portion of the canvas or object.

********************************************************************************

There are other ways also to draw on canvas:

this begins with this command:

```
ctx.beginPath();
ctx.moveTo(10,10);  // moves pen location to 10,10
ctx.lineTo(50,50);  // draw from previous location to 50,50
ctx.lineTo(50,10);
ctx.lineTo(10,50);
//ctx.fill();
//ctx.stroke();
```

there are more to learn about building path refer to :
http://www.w3.org/TR/2dcontext/#building-paths

********************************************************************************

So far, we’ve been drawing everything using exact coordinates. This is fine for a couple shapes but breaks down if you need to draw a bunch of objects.

Canvas2D allows you to translate (move), rotate, or scale objects.

Scaling

scale(x,y) multiplies the x and y values by a given factor so

ctx.scale(2,3);

will make all values twice as large on the x axis and three times as large on the y axis.

Translation

translate(x,y) moves all subsequent draw commands by x number of pixels on horizontally and y pixels vertically.

ctx.translate(20,40); moves all elements drawn after it 20 pixels to the rights and 40 pixels down.

Rotation

ctx.rotate(angleRadians) rotates an object a certain number of radians (generally) about its center. You probably may have learned about radians in school but here's a handy formula to convert a value from degrees to radians.

radians = degrees * (Math.PI/180)

Don't ask us why everything in Computer Graphics uses radians. We have no idea. :)

Order of operations

You should generally scale objects first, rotate them next, and then finally translate last. There are times when you'd want to rotate around an arbitrary point instead of an object's center, that's out of scope for this lesson.

It’s important to note that whatever transformations apply for all subsequent objects until you reverse them.

---

Every canvas objects contains a stack of drawing states. Stacks are data structures that only let you push new items at one end. When you retrieve an item, it's the last item that was pushed or Last In-First Out(LIFO).

Let's see how this would work in code. Let's say you wanted to draw a couple rectangles in different colors. For this small example, we could get away with reassigning the fillStyle each time instead of using save and restore.


```
var c = document.querySelector("#c");
var ctx = c.getContext("2d");

ctx.fillStyle = "blue";
ctx.fillRect(0,0,50,50);

ctx.fillStyle = "green"
ctx.fillRect(100,100,10,10);

ctx.fillStyle = "blue";
ctx.fillRect(200,10,20,20);
This is better.

var c = document.querySelector("#c");
var ctx = c.getContext("2d");

ctx.fillStyle = "blue";
ctx.fillRect(0,0,50,50);

// Save state with blue fill
ctx.save();
ctx.fillStyle = "green";
ctx.fillRect(100,100,10,10);
// Restore to blue fill
ctx.restore();

ctx.fillRect(200,10,20,20);
The canvas state can store:
```

The current transformation matrix (rotation, scaling, translation)
strokeStyle
fillStyle
font
globalAlpha
lineWidth
lineCap
lineJoin
miterLimits
shadowOffsetX
shadowOffsetY
shadowBlur
shadowColor
globalCompositeOperation
textAlign
textBaseline
The current clipping region

---

to select fill and stroke colors do :
ctx.fillstype = "blue";
ctx.strokeStyle = "greem";

we need to define this before we draw otherwise the color wont be selected.

---

to get colors to fill in canvas use hexadecimal or 140 colornames:
https://en.wikipedia.org/wiki/Web_colors

---
