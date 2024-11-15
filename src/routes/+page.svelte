<script lang="ts">
	import ChartWidget from '$lib/ChartWidget.svelte';
	import SellDropProtection from '$lib/SellDropProtection.svelte';
	import '../app.css';
	import { type ComponentType } from 'svelte';

	interface $$Props {
		children?: ComponentType | string;
	}

	export let children: $$Props['children'] = undefined;
</script>

<svelte:head>
	<link
		href="https://fonts.googleapis.com/css2?family=Outfit:wght@500;600;700&display=swap"
		rel="stylesheet"
	/>
</svelte:head>

<div class="layout">
	<header>
		<div class="brand">PROFIT INSURANCE</div>
		<button class="connect-wallet">Connect Wallet</button>
	</header>

	<div class="flex">
		<ChartWidget></ChartWidget>
		<SellDropProtection></SellDropProtection>
	</div>
</div>

<style>
	:global(body) {
		margin: 0;
		font-family: Arial, sans-serif;
		background-color: #8ec5fc;
		background-image: linear-gradient(62deg, #8ec5fc 0%, #e0c3fc 100%);
		min-height: 100vh;
	}

	.layout {
		display: flex;
		flex-direction: column;
		min-height: 100vh;
	}

	header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 1rem 2rem;
		background-color: rgba(255, 255, 255, 0.2);
		backdrop-filter: blur(10px);
		position: sticky;
		top: 0;
		z-index: 10;
	}

	.brand {
		font-family: 'Outfit', sans-serif;
		font-size: 1.5rem;
		font-weight: 700;
		letter-spacing: 0.05em;
		color: #1a1a1a;
		text-transform: uppercase;
	}

	.connect-wallet {
		background-color: #333;
		color: #fff;
		border: none;
		padding: 0.5rem 1rem;
		border-radius: 5px;
		cursor: pointer;
		font-size: 1rem;
		transition: background-color 0.2s ease;
	}

	.connect-wallet:hover {
		background-color: #555;
	}

	main {
		padding: 2rem;
		text-align: center;
		color: #333;
		flex: 1;
	}
</style>
