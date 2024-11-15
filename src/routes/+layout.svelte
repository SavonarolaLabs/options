<script lang="ts">
	import '../app.css';
	let { children } = $props();
</script>

<div class="gradient-wrapper">
	{@render children()}
</div>

<style>
	.gradient-wrapper {
		min-height: 100vh;
		background-color: #2b2b2b;
		background-image: linear-gradient(62deg, #8ec5fc 0%, #e0c3fc 100%);
	}
</style>
