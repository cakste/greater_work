<template>
	<div id="home">
		<div style="left: 0px; top: 0px; width:100%; height: 100%;">
			<img alt="Greater Than Logo Straight From The Source" src="https://greaterthan.eu/wp-content/themes/greaterthan/assets/img/logo.svg">
			<Table />
		</div>
		<div style="position: absolute; z-index: -1; left: 0px; top: 0px; width:100%; height:100%; background-color: #ffffff;">
			<Dots />
		</div>
	</div>
</template>

<script>
import Table from './Table.vue'
import Dots from './Dots.vue'

export default {
	name: 'HomePage',
	components: {
		Table,
		Dots,
	},
	data() {
		return {
			completed: false
		}
	},
}

</script>

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
