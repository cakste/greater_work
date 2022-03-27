<template>
	<!---
	<div class="Home">
		<img alt="Greater Than Logo Straight From The Source" src="https://greaterthan.eu/wp-content/themes/greaterthan/assets/img/logo.svg">
		<Table />

	</div>
	-->
	<div id="app">
		<!--<div style=" left: 0px; top: 0px; width:100%; height: 100%; background: yellow">
		Had this for both logo and table, might not need. Only for dots.
		-->
		<div style="left: 0px; top: 0px; width:100%; height: 100%;">
			<img alt="Greater Than Logo Straight From The Source" src="https://greaterthan.eu/wp-content/themes/greaterthan/assets/img/logo.svg">
			<Table />
		</div>
		<div style="position: absolute; z-index: -1; left: 0px; top: 0px; width:100%; height:100%; background-color: #ffffff;">
			<Dots2 />
		</div>
	</div>
</template>

<!--// Access a child components variables? Nice to include their message. -->

<script>
import Table from './Table.vue'
import Dots2 from './Dots2.vue'

export default {
	name: 'HelloWorld',
	components: {
		Table,
		Dots2,
	},
	props: {
	},

	data() {
		return {
			completed: false
		}
	},

	async mounted() {
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
		/*
		var canvas = document.getElementById("canvas");
		var ctx = canvas.getContext("2d");
		this.vueCanvas = ctx;
		console.log(this.vueCanvas)

		canvas.height = window.innerHeight;
		canvas.width = window.innerWidth;
		console.log(canvas.height, canvas.width)

		ctx.fillStyle = "#42b983"
		ctx.arc(600,200,50,0, 3.14*2);
		ctx.fill()
		*/
	},

	methods: {

	},

}

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

.alt {
	margin-top: 60px;
}
</style>
