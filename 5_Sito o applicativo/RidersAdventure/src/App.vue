<template>
  <div>
	  <Nav />
	  <router-view></router-view>
  </div>
</template>

<script>
	import Nav from "./components/Nav.vue"

	export default {
		components: {
			Nav
		}
	}
</script>

<style>
	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
		height: 89vh;
	}
</style>