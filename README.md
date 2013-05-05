HTML, CSS, JavaScript, and jQuery cheat sheet
=====

http://forresto.github.com/web-interactive-workshop/

!Processing (not Processing)
=====

Processing was great when it was hard to draw on the screen, but now with the HTML ```<canvas>``` it isn’t so hard. Processing.js comes with a lot of baggage that we don’t really need. Here is some pure JavaScript that will get you started drawing on the 2D canvas (almost) as easily as with Processing.

There are ontouchmove, ontouchstart, and ontouchend events if you want your canvas animation to be interactive on touchscreens like iOS.

Examples
* [paint splash](http://forresto.github.com/web-interactive-workshop/fireworks.html)
* [one-button game](http://forresto.github.com/web-interactive-workshop/one-button-game.html)

This is not a library: just use the parts you need!

``` javascript
/*
 *  The notProcessing <canvas> notLibrary! 
 */

var canvas = document.getElementById("drawing");
context = canvas.getContext("2d");

// p.size(400, 400);
canvas.width = 400;
canvas.height = 400;

// If you want to use mouseX and mouseY
var mouseX = 0;
var mouseY = 0;
canvas.onmousemove = function (e) {
  mouseX = e.pageX - canvas.offsetLeft;
  mouseY = e.pageY - canvas.offsetTop;
};

// If you want to use mousePressed
var mousePressed = false;
canvas.onmousedown = function (e) {
  mousePressed = true;
};
canvas.onmouseup = function (e) {
  mousePressed = false;
};

// If you want to use these mouse variables with touchscreens as well
canvas.ontouchmove = function (e) {
  // Don't scroll
  e.preventDefault();

  // Just looks at first finger
  mouseX = e.targetTouches[0].pageX - canvas.offsetLeft;
  mouseY = e.targetTouches[0].pageY - canvas.offsetTop;
}
canvas.ontouchstart = function (e) {
  // Don't select
  e.preventDefault();

  mousePressed = true;
  canvas.ontouchmove(e);
};
canvas.ontouchend = function (e) {
  // Don't select
  e.preventDefault();

  mousePressed = false;
};

// Keyboard interaction
var key;
var keyPressed = false;
var SPACE = 32;
var LEFT = 37;
var UP = 38;
var RIGHT = 39;
var DOWN = 40;
document.onkeydown = function (e) {
    key = e.keyCode;
    keyPressed = true;
};
document.onkeyup = function (e) {
    keyPressed = false;
};


/*
  Fill and stroke color are set like any CSS color. For example:
    "red"
    "#FF0000"
    "rgb(255, 0, 0)"
    "hsl(0, 100%, 50%)"
  Alpha transparency from 0.0 to 1.0:
    "rgba(255, 0, 0, 0.5)"
    "hsla(0, 100%, 50%, 0.5)"
*/

// p.background(0, 0, 0);
context.fillStyle = "#000000";
context.fillRect(0, 0, canvas.width, canvas.height);

// p.stroke(255, 255, 255);
context.strokeStyle = "#FFFFFF";

// p.fill(255, 0, 0);
context.fillStyle = "red";

// p.strokeWeight(6);
context.lineWidth = 6;

// Think τ! https://www.khanacademy.org/math/vi-hart/v/pi-is--still--wrong
var TAU = 2 * Math.PI; 

// p.ellipse(200, 200, 150, 150); (just circles, ellipses are trickier)
context.beginPath();
context.arc(200, 200, 150, 0, TAU, false);
context.fill();
context.stroke();

// Let's make it a function! 
var circle = function (centerX, centerY, radius) {
  context.beginPath();
  context.arc(centerX, centerY, radius, 0, TAU, false);
  context.fill();
  context.stroke();
};

circle(205, 205, 75);



// p.line(x1, y1, x2, y2)
context.beginPath();
context.moveTo(0, 0);
context.lineTo(400, 400);
context.stroke();

// Let's make it a function!
var line = function (x1, y1, x2, y2) {
  context.beginPath();
  context.moveTo(x1, y1);
  context.lineTo(x2, y2);
  context.stroke();
};

// Now it will work like in Processing:
line(200, 100, 100, 200);


// Draw an image
var myImage = new Image();
myImage.onload = function(){
  context.drawImage(myImage, 0, 0);
};
myImage.src = "http://forresto.github.com/web-interactive-workshop/one-button-game-player.jpg"



var x = 0;
var y = 0;
var xSpeed = 5;
var ySpeed = 5;
context.lineWidth = 2;

// draw loop
var draw = function () {
  var hue = Math.random() * 360;

  var size = Math.random() * 28 + 2;
  if (mousePressed) {
    x = mouseX - size / 2;
    y = mouseY - size / 2;
  } else {
    x += xSpeed;
    y += ySpeed;
    if (x > canvas.width) {
      x = canvas.width;
      xSpeed = -xSpeed;
    }
    if (x < 0){
      x = 0;
      xSpeed = -xSpeed;
    }
    if (y > canvas.height) {
      y = canvas.height;
      ySpeed = -ySpeed;
    }
    if (y < 0) {
      y = 0;
      ySpeed = -ySpeed;
    }
  }
  context.fillStyle = "hsla(" + hue + ", 100%, 50%, 0.75)";
  context.fillRect(x, y, size, size);
  hue = (hue + 180) % 360;
  context.strokeStyle = "hsla(" + hue + ", 100%, 50%, 1.0)";
  context.strokeRect(x, y, size, size);

};

// This calls the draw loop at 30fps
setInterval(draw, 1000 / 30);
```


In a fiddle for instant editing: http://jsfiddle.net/forresto/tyKsy/
