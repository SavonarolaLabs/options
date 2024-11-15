<script lang="ts">
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	let servicePrice = '';
	let priceLevel = '';
	let duration: 'week' | 'month' | '3 months' = 'month';
	let collateralAmount = '';

	const submitProtection = () => {
		dispatch('submit', { servicePrice, priceLevel, duration, collateralAmount });
	};

	// Computed values for the summary
	$: collateralSigUSD = parseFloat(collateralAmount) || 0;
	$: pricePerErg = parseFloat(priceLevel) || 0;
	$: earningsErg = parseFloat(servicePrice) || 0;
	$: riskErg = collateralSigUSD * pricePerErg || 0;
</script>

<div class="widget">
	<div class="title">Sell Insurance — Earn ERG</div>

	<div class="field">
		<label class="label" for="servicePrice">Insurance Price</label>
		<div class="input-wrapper">
			<input id="servicePrice" class="input" bind:value={servicePrice} />
			<span class="currency">ERG</span>
		</div>
	</div>

	<div class="field-row">
		<div class="field half">
			<label class="label" for="priceLevel">Price Level</label>
			<div class="input-wrapper">
				<input id="priceLevel" class="input" bind:value={priceLevel} />
				<span class="currency">USD</span>
			</div>
		</div>

		<div class="field half">
			<label class="label" for="collateralAmount">Collateral</label>
			<div class="input-wrapper">
				<input id="collateralAmount" class="input" bind:value={collateralAmount} />
				<span class="currency">SigUSD</span>
			</div>
		</div>
	</div>

	<div class="field">
		<label class="label">Duration</label>
		<div class="toggle-group">
			<div
				class="toggle {duration === 'week' ? 'active' : ''}"
				on:click={() => (duration = 'week')}
			>
				Week
			</div>
			<div
				class="toggle {duration === 'month' ? 'active' : ''}"
				on:click={() => (duration = 'month')}
			>
				Month
			</div>
			<div
				class="toggle {duration === '3 months' ? 'active' : ''}"
				on:click={() => (duration = '3 months')}
			>
				3&nbsp;Months
			</div>
		</div>
	</div>

	<div class="summary">
		<label class="label">Best case</label>
		<div class="summary-text">
			You earn <span class="font-bold text-white">{earningsErg.toFixed(2)} ERG</span> and keep
			<span class="font-bold text-white">{collateralSigUSD.toFixed(2)} SigUSD</span>.
		</div>
		<label class="label mt-4">Worst case</label>
		<div class="summary-text">
			You earn {earningsErg.toFixed(2)} ERG + receive {riskErg.toFixed(2)} ERG.
		</div>
	</div>

	<button class="button" on:click={submitProtection}>Sell</button>
</div>

<style>
	:root {
		--input-bg-color: #2b2b2b;
		--input-text-color: #ffffff;
		--input-placeholder-color: #7a7a7a;
	}

	.widget {
		background-color: #181818 !important;
		color: #d1d1d1 !important;
		padding: 1rem !important;
		border-radius: 10px !important;
		max-width: 400px !important;
		margin: auto !important;
		font-family: Arial, sans-serif !important;
	}
	.title {
		font-size: 1.15rem !important;
		font-weight: 600 !important;
		color: #d1d1d1 !important;
		margin-bottom: 1rem !important;
	}
	.field {
		margin-bottom: 1.5rem !important;
	}
	.field-row {
		display: flex !important;
		gap: 1rem !important;
	}
	.half {
		flex: 1 !important;
	}
	.label {
		font-size: 0.9rem !important;
		color: var(--input-placeholder-color) !important;
		margin-bottom: 0.25rem !important;
		display: block !important;
		font-weight: 500 !important;
	}
	.input-wrapper {
		position: relative !important;
		display: flex !important;
		align-items: center !important;
	}
	.widget .input {
		background-color: var(--input-bg-color) !important;
		color: var(--input-text-color) !important;
		border: none !important;
		padding: 0.75rem !important;
		padding-right: 2.5rem !important;
		width: 100% !important;
		border-radius: 5px !important;
		font-size: 1.2rem !important;
		outline: none !important;
		box-shadow: none !important;
		-webkit-appearance: none !important;
		-moz-appearance: none !important;
	}
	.widget .input::placeholder {
		color: var(--input-placeholder-color) !important;
	}
	.widget .input:focus {
		background-color: var(--input-bg-color) !important;
		color: var(--input-text-color) !important;
	}
	.currency {
		position: absolute !important;
		right: 0.75rem !important;
		font-size: 0.9rem !important;
		color: var(--input-placeholder-color) !important;
	}
	.toggle-group {
		display: flex !important;
		justify-content: space-between !important;
		margin-bottom: 1.5rem !important;
	}
	.toggle {
		flex: 1 !important;
		text-align: center !important;
		padding: 0.75rem !important;
		border-radius: 5px !important;
		cursor: pointer !important;
		background: var(--input-bg-color) !important;
		color: var(--input-text-color) !important;
		margin: 0 0.25rem !important;
		font-size: 0.9rem !important;
		font-weight: 500 !important;
	}
	.toggle.active {
		background: #4caf50 !important;
		color: #000000 !important;
	}
	.summary {
		background-color: #252525 !important;
		color: #b0b0b0 !important;
		padding: 1rem !important;
		border-radius: 5px !important;
		margin-bottom: 1.5rem !important;
	}
	.summary-text {
		font-size: 0.9rem !important;
	}
	.button {
		background-color: #3a82f6 !important;
		color: #ffffff !important;
		width: 100% !important;
		padding: 0.75rem !important;
		border-radius: 5px !important;
		font-weight: 600 !important;
		font-size: 1rem !important;
		cursor: pointer !important;
		text-align: center !important;
	}

	/* Autofill fix for Chrome, Safari, and Firefox using CSS variables */
	.input:-webkit-autofill,
	.input:-webkit-autofill:focus,
	.input:-webkit-autofill:hover,
	.input:-internal-autofill-selected {
		background-color: var(--input-bg-color) !important;
		color: var(--input-text-color) !important;
		-webkit-text-fill-color: var(--input-text-color) !important;
		transition: background-color 0s ease-in-out 0s;
	}
</style>
