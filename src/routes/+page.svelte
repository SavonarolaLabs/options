<script lang="ts">
	import ChartWidget from '$lib/ChartWidget.svelte';
	import OrderTable from '$lib/OrderTable.svelte';
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
		<div class="flex">
			<img style="width:2em;height:auto;" src="/bull.png" alt="" />
			<div class="brand ml-2">HEDGE.EARN</div>
		</div>
		<button class="connect-wallet">Connect Wallet</button>
	</header>

	<div class="content my-20">
		<div class="text-white">
			<div>
				<div class="text-8xl" style="opacity:0.8">
					<div>Hold&nbspSigUSD</div>
					<div>Earn ERG</div>
				</div>
				<div>100% onchain. 100% open-source.</div>
			</div>
			<div style="opacity:0.8">
				Put your stable coins to work.<br /> With HEDGE.EARN you help miners protect the downside risk
				while earning cold hard ERG Coins.
			</div>
		</div>
		<div class="combined-section">
			<div class="chart-container">
				<ChartWidget />
			</div>
			<div class="form-container">
				<SellDropProtection />
			</div>
		</div>
	</div>
	<OrderTable></OrderTable>
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
		transition:
			transform 0.2s ease,
			color 0.2s ease;
	}

	.brand:hover {
		transform: scale(1.05);
		color: #3cff78;
	}

	.connect-wallet {
		background-color: #355c37;
		color: #fff;
		border: none;
		padding: 0.5rem 1rem;
		border-radius: 5px;
		cursor: pointer;
		font-size: 1rem;
		transition: background-color 0.2s ease;
	}

	.connect-wallet:hover {
		background-color: #0f3b1f;
	}

	.content {
		display: flex;
		justify-content: space-around;
		align-items: center;
		padding: 2rem;
		flex-grow: 1;
	}

	.text-white > div {
		text-align: left;
		max-width: 500px;
		margin: 0 auto;
		padding: 1rem;
	}

	.text-8xl {
		font-weight: 800;
	}

	.text-white > div > div:last-child {
		font-size: 1.25rem;
		font-weight: 600;
		color: #a8ff9d;
		margin-top: 1rem;
	}

	.combined-section {
		display: flex;
		background-color: #1818185e;
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
		height: 490px;
	}

	main {
		padding: 2rem;
		text-align: center;
		color: #e0e0e0;
		flex: 1;
	}
</style>
