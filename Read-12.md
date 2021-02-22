Charts :

Charts are far better for displaying data visually than tables and have the added benefit that no one is ever going to press-gang them into use as a layout tool. They’re easier to look at and convey data quickly, but they’re not always easy to create.

A great way to get started with charts is with Chart.js, a JavaScript plugin that uses HTML5’s canvas element to draw the graph onto the page. It’s a well documented plugin that makes using all kinds of bar charts, line charts, pie charts and more, incredibly easy.

To see how to use chart.js we’re going to create a set of 3 graphs; one will show the number of buyers a fictional product has over the course of 6 months, this will be a line chart; the second will show which countries the customers come from, this will be the pie chart; finally we’ll use a bar chart to show profit over the period.

Let's start this tutorial by looking at the <canvas> HTML element itself. At the end of this page, you will know how to set up a canvas 2D context and have drawn a first example in your browser.

The <canvas> element
<canvas id="tutorial" width="150" height="150"></canvas>
At first sight a <canvas> looks like the <img> element, with the only clear difference being that it doesn't have the src and alt attributes. Indeed, the <canvas> element has only two attributes, width and height. These are both optional and can also be set using DOM properties. When no width and height attributes are specified, the canvas will initially be 300 pixels wide and 150 pixels high. The element can be sized arbitrarily by CSS, but during rendering the image is scaled to fit its layout size: if the CSS sizing doesn't respect the ratio of the initial canvas, it will appear distorted.

Note: If your renderings seem distorted, try specifying your width and height attributes explicitly in the <canvas> attributes, and not using CSS.

The id attribute isn't specific to the <canvas> element but is one of the global HTML attributes which can be applied to any HTML element (like class for instance). It is always a good idea to supply an id because this makes it much easier to identify it in a script.

The <canvas> element can be styled just like any normal image (margin, border, background…). These rules, however, don't affect the actual drawing on the canvas. We'll see how this is done in a dedicated chapter of this tutorial. When no styling rules are applied to the canvas it will initially be fully transparent.

Fallback content
The <canvas> element differs from an <img> tag in that, like for <video>, <audio>, or <picture> elements, it is easy to define some fallback content, to be displayed in older browsers not supporting it, like versions of Internet Explorer earlier than version 9 or textual browsers. You should always provide fallback content to be displayed by those browsers.

Providing fallback content is very straightforward: just insert the alternate content inside the <canvas> element. Browsers that don't support <canvas> will ignore the container and render the fallback content inside it. Browsers that do support <canvas> will ignore the content inside the container, and just render the canvas normally.

For example, we could provide a text description of the canvas content or provide a static image of the dynamically rendered content. This can look something like this:

<canvas id="stockGraph" width="150" height="150">
  current stock price: $3.15 + 0.15
</canvas>

<canvas id="clock" width="150" height="150">
  <img src="images/clock.png" width="150" height="150" alt=""/>
</canvas>
Telling the user to use a different browser that supports canvas does not help users who can't read the canvas at all, for example. Providing a useful fallback text or sub DOM helps to make the canvas more accessible.

Required </canvas> tag
As a consequence of the way fallback is provided, unlike the <img> element, the <canvas> element requires the closing tag (</canvas>). If this tag is not present, the rest of the document would be considered the fallback content and wouldn't be displayed.

If fallback content is not needed, a simple <canvas id="foo" ...></canvas> is fully compatible with all browsers that support canvas at all.

The rendering context
The <canvas> element creates a fixed-size drawing surface that exposes one or more rendering contexts, which are used to create and manipulate the content shown. In this tutorial, we focus on the 2D rendering context. Other contexts may provide different types of rendering; for example, WebGL uses a 3D context based on OpenGL ES.

The canvas is initially blank. To display something, a script first needs to access the rendering context and draw on it. The <canvas> element has a method called getContext(), used to obtain the rendering context and its drawing functions. getContext() takes one parameter, the type of context. For 2D graphics, such as those covered by this tutorial, you specify "2d" to get a CanvasRenderingContext2D.

var canvas = document.getElementById('tutorial');
var ctx = canvas.getContext('2d');
The first line in the script retrieves the node in the DOM representing the <canvas> element by calling the document.getElementById() method. Once you have the element node, you can access the drawing context using its getContext() method.

Checking for support
The fallback content is displayed in browsers which do not support <canvas>. Scripts can also check for support programmatically by testing for the presence of the getContext() method. Our code snippet from above becomes something like this:

var canvas = document.getElementById('tutorial');

if (canvas.getContext) {
  var ctx = canvas.getContext('2d');
  // drawing code here
} else {
  // canvas-unsupported code here
}
A skeleton template
Here is a minimalistic template, which we'll be using as a starting point for later examples.

Note: it is not good practice to embed a script inside HTML. We do it here to keep the example concise.

<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8"/>
    <title>Canvas tutorial</title>
    <script type="text/javascript">
      function draw() {
        var canvas = document.getElementById('tutorial');
        if (canvas.getContext) {
          var ctx = canvas.getContext('2d');
        }
      }
    </script>
    <style type="text/css">
      canvas { border: 1px solid black; }
    </style>
  </head>
  <body onload="draw();">
    <canvas id="tutorial" width="150" height="150"></canvas>
  </body>
</html>
The script includes a function called draw(), which is executed once the page finishes loading; this is done by listening for the load event on the document. This function, or one like it, could also be called using window.setTimeout(), window.setInterval(), or any other event handler, as long as the page has been loaded first.

Here is how a template would look in action. As shown here, it is initially blank.



A simple example
To begin, let's take a look at a simple example that draws two intersecting rectangles, one of which has alpha transparency. We'll explore how this works in more detail in later examples.

<!DOCTYPE html>
<html>
 <head>
  <meta charset="utf-8"/>
  <script type="application/javascript">
    function draw() {
      var canvas = document.getElementById('canvas');
      if (canvas.getContext) {
        var ctx = canvas.getContext('2d');

        ctx.fillStyle = 'rgb(200, 0, 0)';
        ctx.fillRect(10, 10, 50, 50);

        ctx.fillStyle = 'rgba(0, 0, 200, 0.5)';
        ctx.fillRect(30, 30, 50, 50);
      }
    }
  </script>
 </head>
 <body onload="draw();">
   <canvas id="canvas" width="150" height="150"></canvas>
 </body>
</html>
This example looks like this:

Screenshot	Live sample
	
Drawing shapes with canvas
Web technology for developers
Web APIs
Canvas API
Canvas tutorial
Drawing shapes with canvas
Select your preferred language
English (US)
Change language
Table of contents
« Previous
Next »
Now that we have set up our canvas environment, we can get into the details of how to draw on the canvas. By the end of this article, you will have learned how to draw rectangles, triangles, lines, arcs and curves, providing familiarity with some of the basic shapes. Working with paths is essential when drawing objects onto the canvas and we will see how that can be done.

The grid
Before we can start drawing, we need to talk about the canvas grid or coordinate space. Our HTML skeleton from the previous page had a canvas element 150 pixels wide and 150 pixels high. To the right, you see this canvas with the default grid overlayed. Normally 1 unit in the grid corresponds to 1 pixel on the canvas. The origin of this grid is positioned in the top left corner at coordinate (0,0). All elements are placed relative to this origin. So the position of the top left corner of the blue square becomes x pixels from the left and y pixels from the top, at coordinate (x,y). Later in this tutorial we'll see how we can translate the origin to a different position, rotate the grid and even scale it, but for now we'll stick to the default.

Drawing rectangles
Unlike SVG, <canvas> only supports two primitive shapes: rectangles and paths (lists of points connected by lines). All other shapes must be created by combining one or more paths. Luckily, we have an assortment of path drawing functions which make it possible to compose very complex shapes.

First let's look at the rectangle. There are three functions that draw rectangles on the canvas:

fillRect(x, y, width, height)
Draws a filled rectangle.
strokeRect(x, y, width, height)
Draws a rectangular outline.
clearRect(x, y, width, height)
Clears the specified rectangular area, making it fully transparent.
Each of these three functions takes the same parameters. x and y specify the position on the canvas (relative to the origin) of the top-left corner of the rectangle. width and height provide the rectangle's size.

Below is the draw() function from the previous page, but now it is making use of these three functions.

Rectangular shape example
function draw() {
  var canvas = document.getElementById('canvas');
  if (canvas.getContext) {
    var ctx = canvas.getContext('2d');

    ctx.fillRect(25, 25, 100, 100);
    ctx.clearRect(45, 45, 60, 60);
    ctx.strokeRect(50, 50, 50, 50);
  }
}
This example's output is shown below.

Screenshot	Live sample
	
The fillRect() function draws a large black square 100 pixels on each side. The clearRect() function then erases a 60x60 pixel square from the center, and then strokeRect() is called to create a rectangular outline 50x50 pixels within the cleared square.

In upcoming pages we'll see two alternative methods for clearRect(), and we'll also see how to change the color and stroke style of the rendered shapes.

Unlike the path functions we'll see in the next section, all three rectangle functions draw immediately to the canvas.

Drawing paths
Now let's look at paths. A path is a list of points, connected by segments of lines that can be of different shapes, curved or not, of different width and of different color. A path, or even a subpath, can be closed. To make shapes using paths, we take some extra steps:

First, you create the path.
Then you use drawing commands to draw into the path.
Once the path has been created, you can stroke or fill the path to render it.
Here are the functions used to perform these steps:

beginPath()
Creates a new path. Once created, future drawing commands are directed into the path and used to build the path up.
Path methods
Methods to set different paths for objects.
closePath()
Adds a straight line to the path, going to the start of the current sub-path.
stroke()
Draws the shape by stroking its outline.
fill()
Draws a solid shape by filling the path's content area.
The first step to create a path is to call the beginPath(). Internally, paths are stored as a list of sub-paths (lines, arcs, etc) which together form a shape. Every time this method is called, the list is reset and we can start drawing new shapes.

Note: When the current path is empty, such as immediately after calling beginPath(), or on a newly created canvas, the first path construction command is always treated as a moveTo(), regardless of what it actually is. For that reason, you will almost always want to specifically set your starting position after resetting a path.
The second step is calling the methods that actually specify the paths to be drawn. We'll see these shortly.

The third, and an optional step, is to call closePath(). This method tries to close the shape by drawing a straight line from the current point to the start. If the shape has already been closed or there's only one point in the list, this function does nothing.

Note: When you call fill(), any open shapes are closed automatically, so you don't have to call closePath(). This is not the case when you call stroke().
Drawing a triangle
For example, the code for drawing a triangle would look something like this:

function draw() {
  var canvas = document.getElementById('canvas');
  if (canvas.getContext) {
    var ctx = canvas.getContext('2d');

    ctx.beginPath();
    ctx.moveTo(75, 50);
    ctx.lineTo(100, 75);
    ctx.lineTo(100, 25);
    ctx.fill();
  }
}
The result looks like this:

Screenshot	Live sample
	
