<script lang="ts">
	import SortArrow from './SortArrow.svelte';

	// Define the structure for the table row data
	interface Option {
		optionType: string;
		strikePrice: number;
		premium: number;
		collateral: number;
		expiration: string;
	}

	// Generate random expiration values based on your widget's durations
	function getRandomExpiration(): string {
		const durations = ['1 Week', '2 Weeks', '1 Month', '3 Months'];
		return durations[Math.floor(Math.random() * durations.length)];
	}

	// Generate random dummy data
	const options: Option[] = [
		{
			optionType: 'Put',
			strikePrice: 1.3,
			premium: 50.0,
			collateral: 500.0,
			expiration: getRandomExpiration()
		},
		{
			optionType: 'Put',
			strikePrice: 1.25,
			premium: 40.0,
			collateral: 400.0,
			expiration: getRandomExpiration()
		},
		{
			optionType: 'Put',
			strikePrice: 1.2,
			premium: 60.0,
			collateral: 600.0,
			expiration: getRandomExpiration()
		},
		{
			optionType: 'Put',
			strikePrice: 1.15,
			premium: 30.0,
			collateral: 300.0,
			expiration: getRandomExpiration()
		},
		{
			optionType: 'Put',
			strikePrice: 1.1,
			premium: 45.0,
			collateral: 450.0,
			expiration: getRandomExpiration()
		}
	];
</script>

<div class="table-wrapper">
	<table class="order-book">
		<thead>
			<tr>
				<th>
					<div class="flex items-center gap-2">
						Option Type <SortArrow />
					</div>
				</th>
				<th>
					<div class="flex items-center gap-2">
						Strike Price (USD) <SortArrow />
					</div>
				</th>
				<th>
					<div class="flex items-center gap-2">
						Premium (ERG) <SortArrow />
					</div>
				</th>
				<th>
					<div class="flex items-center gap-2">
						Collateral (SigUSD) <SortArrow />
					</div>
				</th>
				<th>
					<div class="flex items-center gap-2">
						Expiration <SortArrow />
					</div>
				</th>
			</tr>
		</thead>
		<tbody>
			{#each options as option}
				<tr>
					<td>{option.optionType}</td>
					<td>{option.strikePrice.toFixed(2)}</td>
					<td>{option.premium.toFixed(2)}</td>
					<td>{option.collateral.toFixed(2)}</td>
					<td>{option.expiration}</td>
				</tr>
			{/each}
		</tbody>
	</table>
</div>

<style>
	.table-wrapper {
		overflow-x: auto;
		margin-left: 100px;
		margin-right: 100px;
		padding-left: 1em;
		/* background-color: #181818; */
		background-color: #1818185e;
		border-radius: 0.8em;
	}

	.order-book {
		width: 100%;
		border-collapse: collapse;
		color: #ffffff;
		font-family: Arial, sans-serif;
	}
	.order-book th {
		text-align: left;
		padding: 10px;
		font-weight: 600;
		color: #d1d1d1;
	}
	.order-book td {
		padding: 10px;
		color: #ffffff;
		font-size: 0.9rem;
	}
</style>
