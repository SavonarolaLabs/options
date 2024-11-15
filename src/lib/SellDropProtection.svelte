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
			You earn {earningsErg.toFixed(2)} ERG and keep {collateralSigUSD.toFixed(2)} SigUSD.
		</div>
		<label class="label mt-4">Worst case</label>
		<div class="summary-text">
			You earn {earningsErg.toFixed(2)} ERG + receive {riskErg.toFixed(2)} ERG.
		</div>
	</div>

	<button class="button" on:click={submitProtection}>Sell</button>
</div>

<style>
	.widget {
		background-color: #181818;
		color: #d1d1d1;
		padding: 1rem;
		border-radius: 10px;
		max-width: 400px;
		margin: auto;
		font-family: Arial, sans-serif;
	}
	.title {
		font-size: 1.15rem;
		font-weight: 600;
		color: #d1d1d1;
		margin-bottom: 1rem;
	}
	.field {
		margin-bottom: 1.5rem;
	}
	.field-row {
		display: flex;
		gap: 1rem;
	}
	.half {
		flex: 1;
	}
	.label {
		font-size: 0.9rem;
		color: #7a7a7a;
		margin-bottom: 0.25rem;
		display: block;
		font-weight: 500;
	}
	.input-wrapper {
		position: relative;
		display: flex;
		align-items: center;
	}
	.input {
		background: #2b2b2b;
		color: #fff;
		border: none;
		padding: 0.75rem;
		padding-right: 2.5rem;
		width: 100%;
		border-radius: 5px;
		font-size: 1.2rem;
		/* Ensuring the background remains dark on input focus and value change */
		appearance: none;
		outline: none;
	}
	.input:focus {
		background: #2b2b2b; /* Keeps the same dark background on focus */
	}
	.currency {
		position: absolute;
		right: 0.75rem;
		font-size: 0.9rem;
		color: #7a7a7a;
	}
	.toggle-group {
		display: flex;
		justify-content: space-between;
		margin-bottom: 1.5rem;
	}
	.toggle {
		flex: 1;
		text-align: center;
		padding: 0.75rem;
		border-radius: 5px;
		cursor: pointer;
		background: #2b2b2b;
		color: #d1d1d1;
		margin: 0 0.25rem;
		font-size: 0.9rem;
		font-weight: 500;
	}
	.toggle.active {
		background: #4caf50;
		color: #000000;
	}
	.summary {
		background-color: #252525;
		color: #b0b0b0;
		padding: 1rem;
		border-radius: 5px;
		margin-bottom: 1.5rem;
	}
	.summary-text {
		font-size: 0.9rem;
	}
	.button {
		background-color: #3a82f6;
		color: #ffffff;
		width: 100%;
		padding: 0.75rem;
		border-radius: 5px;
		font-weight: 600;
		font-size: 1rem;
		cursor: pointer;
		text-align: center;
	}
</style>
