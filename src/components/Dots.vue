<template>
	<div class="hello">
	</div>
	<canvas class="dots">Your browser does not support canvas.</canvas>
	<h1>skdl</h1>
</template>

<!--// Access a child components variables? Nice to include their message. -->

<script>

export default {
	name: 'HelloWorld',
	components: {

	},
	props: {
		msg: String
	},

	data() {
		return {
			count: 1,
			APIData: [],
			exitCode: "STILL FETCHING...",
			testing: [],
			completed: false
		}
	},

	mounted() {
		// Try Async
		/*
		const response = await fetch("http://dummy.restapiexample.com/api/v1/employees");
		const tmp = await response.data;
		if (response.status == 429) {
			this.exitCode = "429";
			this.completed = false
		}
		else {
			this.exitCode = "OK!"
			console.log(tmp)
			this.completed = true
		}
		this.APIData = tmp;
		*/

		/* //Not Async
		fetch("http://dummy.restapiexample.com/api/v1/employees")
			.then(response => {
			this.APIData = response.data;
			console.log(this.APIData)
			})
			.catch(error => {
				alert(error)
			})
			this.testing = typeof(this.APIData)
		*/

		// ----------------------------------------

		var canvas = 'canvas.dots';
		var context = canvas[0].getContext('2d');
		var canvasWidth = canvas.width();
		var canvasHeight = canvas.height();
		let i;

		console.log()
		canvas.attr({height: canvasHeight, width: canvasWidth});

		// Set an array of dot objects.
		var dots = [
			{ x: 100, y: 100, radius: 25, xMove: '+', yMove: '+' },
			{ x: 40, y: 200, radius: 25, xMove: '-', yMove: '+' },
			{ x: 250, y: 300, radius: 25, xMove: '+', yMove: '-' },
			{ x: 150, y: 35, radius: 25, xMove: '-', yMove: '-' }
		];

		// Notice in the moveDot function we can make dots go faster if we increment
		// by more than 1 pixel each time.
		var frameLength = 2;

		// Draw each dot in the dots array.
		for( i = 0; i < dots.length; i++ ) {
			this.drawDot(dots[i]);
		}

		window.requestAnimationFrame(moveDot);




		// Render it again
		window.requestAnimationFrame(moveDot);
		}

	},

	methods: {
		increment() {
			this.count++
		},
		drawDot(dot) {
			this.context.beginPath();
			this.context.arc(dot.x, dot.y, dot.radius, 0, 2 * Math.PI, false);
			this.context.fillStyle = '#F03C69';
			this.context.fill();
		},
		moveDot() {
			context.clearRect(0, 0, canvasWidth, canvasHeight)
			// Iterate over all the dots.
			for(let i = 0; i < dots.length; i++ ) {

				if( dots[i].xMove == '+' ) {
					dots[i].x += frameLength;
				} else {
					dots[i].x -= frameLength;
				}
				if( dots[i].yMove == '+' ) {
					dots[i].y += frameLength;
				} else {
					dots[i].y -= frameLength;
				}

				this.drawDot(dots[i])

				if( (dots[i].x + dots[i].radius) >= canvasWidth ) {
					dots[i].xMove = '-';
				}
				if( (dots[i].x - dots[i].radius) <= 0 ) {
					dots[i].xMove = '+';
				}
				if( (dots[i].y + dots[i].radius) >= canvasHeight ) {
					dots[i].yMove = '-';
				}
				if( (dots[i].y - dots[i].radius) <= 0 ) {
					dots[i].yMove = '+';
				}
			}
	},

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

.hello {
	margin: 60px 0px 0px 0px
}
</style>
