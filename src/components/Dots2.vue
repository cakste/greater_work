<template>

	<canvas id="canvas"></canvas>
</template>

<!--// Access a child components variables? Nice to include their message. -->

<script>

export default {
	name: 'backgroundDots',
	components: {

	},

	data() {
		return {
			count: 1,
			vueCanvas: null,
			ctx: null,
			dots: null,
			width: 0,
			height: 0,
		}
	},

	mounted() {
		var canvas = document.getElementById("canvas");
		this.ctx = canvas.getContext("2d");
		this.vueCanvas = this.ctx;

		canvas.height = window.innerHeight;
		canvas.width = window.innerWidth;

		// Create random dots.
		this.dots = []
		var radius = 25;
		var signs = ['-', '+']
		for (let i = 0; i < 8; i++) {
			var dot = {};
			dot['x'] = Math.floor(Math.random()*(window.innerWidth-2*radius)) + radius
			dot['y'] = Math.floor(Math.random()*(window.innerHeight-2*radius)) + radius
			console.log(window.innerHeight)
			dot['xMove'] = signs[Math.floor(Math.random()*2)]
			dot['yMove'] = signs[Math.floor(Math.random()*2)]
			dot['radius'] = radius
			this.dots.push(dot)
		}
		console.log(this.dots)

		// Draw each dot in the dots array.
		for (let i = 0; i < this.dots.length; i++) {
			this.drawDot(this.dots[i]);
		}

		// Continue to update dots. Update canvas size when window changes size.
		window.setInterval(() => {
			document.getElementById("canvas").width = window.innerWidth
			document.getElementById("canvas").height = window.innerHeight
			this.moveDot()
		}, 15)

		//Get to canvas css?canvas.attr({height: canvas.height, width: canvas.width});
		/*
		var canvas = $('canvas.dots');
		//var context = canvas[0].getContext('2d');
		var canvasWidth = canvas.width();
		var canvasHeight = canvas.height();
		let i;

		console.log(canvasWidth)
		canvas.attr({height: canvasHeight, width: canvasWidth});





		//window.requestAnimationFrame(moveDot);




		// Render it again
		//window.requestAnimationFrame(moveDot);
		*/
		
	},

	methods: {
		drawDot(dot) {
			this.ctx.beginPath();
			this.ctx.arc(dot.x, dot.y, dot.radius, 0, 2 * Math.PI, false);
			this.ctx.fillStyle = '#1e0072';
			this.ctx.fill();
		},
		moveDot() {

			var dots = this.dots
			var frameLength = 2;

			this.ctx.clearRect(0, 0, window.outerWidth, window.outerHeight)

			// Iterate over all the dots.
			for(let i = 0; i < dots.length; i++ ) {

				if( dots[i].xMove == '+' ) {
					dots[i].x += frameLength
				} else {
					dots[i].x -= frameLength
				}
				if( dots[i].yMove == '+' ) {
					dots[i].y += frameLength
				} else {
					dots[i].y -= frameLength
				}

				this.drawDot(dots[i])

				if ((dots[i].x + dots[i].radius) >= window.innerWidth ) {
					dots[i].xMove = '-';
				}
				if ((dots[i].x - dots[i].radius) <= 0 ) {
					dots[i].xMove = '+';
				}
				if ((dots[i].y + dots[i].radius) >= window.innerHeight ) {
					dots[i].yMove = '-';
				}
				if ((dots[i].y - dots[i].radius) <= 0 ) {
					dots[i].yMove = '+';
				}
			}
		},
	}
}




// ----------------------------------------


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}


</style>
