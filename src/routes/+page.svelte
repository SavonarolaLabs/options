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
		<div class="brand">HEDGE</div>
		<button class="connect-wallet">Connect Wallet</button>
	</header>

	<div class="content">
		<div class="combined-section">
			<div class="chart-container">
				<ChartWidget />
			</div>
			<div class="form-container">
				<SellDropProtection />
			</div>
		</div>
	</div>
</div>

<style>
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
		background-color: #1e1e1e;
		backdrop-filter: blur(5px);
		box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.25);
		position: sticky;
		top: 0;
		z-index: 10;
	}

	.brand {
		font-family: 'Outfit', sans-serif;
		font-size: 1.5rem;
		font-weight: 700;
		letter-spacing: 0.05em;
		color: #ffffff;
		text-transform: uppercase;
	}

	.connect-wallet {
		background-color: #2b2b2b;
		color: #fff;
		border: none;
		padding: 0.5rem 1rem;
		border-radius: 5px;
		cursor: pointer;
		font-size: 1rem;
		transition: background-color 0.2s ease;
	}

	.connect-wallet:hover {
		background-color: #16a34a;
	}

	.content {
		display: flex;
		justify-content: center;
		align-items: center;
		padding: 2rem;
	}

	.combined-section {
		display: flex;
		background-color: #181818;

		border-radius: 12px;
		padding: 1.5rem;
		box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.15);
		gap: 1rem;
	}

	.chart-container,
	.form-container {
		width: 100%;
		max-width: 500px;
	}

	.chart-container {
		max-width: 600px;
	}

	main {
		padding: 2rem;
		text-align: center;
		color: #e0e0e0;
		flex: 1;
	}
</style>
