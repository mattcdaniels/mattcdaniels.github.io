---
title: Julia Set
layout: page
---

## Visualing Julia sets with Javascript


Real component: <input type="text" id="realpart" value = "0.3"><br>
Imaginary component: <input type="text" id="imaginarypart" value ="0.5"><br>
Image Size: <input type="text" id="size" value ="500"><br>

<div>
	<canvas id="vis" width="500" height="500"></canvas>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjs/5.2.3/math.js" type="text/javascript"></script>

<script>
var visualization = document.getElementById('vis');

var realpart = document.getElementById("realpart");
var imaginarypart = document.getElementById("imaginarypart");
var size = document.getElementById("size");

size.addEventListener("change", resize);
realpart.addEventListener("change",reDraw);
imaginarypart.addEventListener("change", reDraw);


function resize () {
	visualization.width = size.value;
	visualization.height = size.value;
	reDraw()
}


function reDraw() {
	var context = visualization.getContext('2d');

	var figWidth = visualization.width;
	var figHeight = visualization.height;

	var c = math.complex(realpart.value,imaginarypart.value)
	var R = (1+Math.sqrt(1+4*c.toPolar().r))*1.0/2

	var somedata = new ImageData(figWidth,figHeight);
	var row;
	var column;
	var x;
	var y;
	var width = somedata.width;
	var height = somedata.height;
	       
	for(var i = 0; i < somedata.data.length/4; i++) {
	row = i / width;
	column = i % width;
	x = R - (row*1.0/height) * 2 * R;
	y = 2.0 * R * (column*1.0/width) - R;

	var distance = 0;
	var iter = 0;
	var maxiter = 50;
	var position = math.complex(x,y);

	while(iter < maxiter) {
	  position = math.multiply(position, position);
	  position = math.add(position, c);
	  distance = position.toPolar().r;
	  somedata.data[i*4+3] = 255;
	  if (distance > R) {
	      somedata.data[i*4] = (1-iter/maxiter) * 255;
	      somedata.data[i*4+1] = 255;
	      somedata.data[i*4+2] = (1-iter/maxiter) * 255;
	      somedata.data[i*4+3] = 255;
	      break;
	      }
	  iter++;
	}

	}

	context.putImageData(somedata,0,0);
	context.canvas;
}

reDraw()

</script>

-------