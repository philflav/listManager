<script>
	import Nav from '../components/nav.svelte';
	import '../styles/global.css';
</script>

<div class="container pt-16 py-2text-center">
	<Nav />
	<slot />
</div>

<footer class="container py-20 text-center">Copyright: Phil Flavin 2022</footer>

<style>
	@tailwind base;
	@tailwind components;
	@tailwind utilities;
</style>
