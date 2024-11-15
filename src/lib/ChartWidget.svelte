<script lang="ts">
	import { onMount, onDestroy } from 'svelte';
	import { createChart, ColorType } from 'lightweight-charts';
	import type { IChartApi, CandlestickData } from 'lightweight-charts';

	let chartContainer: HTMLDivElement;
	let chart: IChartApi;
	let candleSeries: any;
	let currentPrice = 0;
	let priceChange = 0;
	let priceChangePercent = 0;
	let updateInterval: NodeJS.Timeout;
	let lastUpdate = '';

	// Store candle data
	let candles: CandlestickData[] = [];

	async function fetchCurrentPrice() {
		try {
			const response = await fetch(
				'https://api.coingecko.com/api/v3/simple/price?ids=ergo&vs_currencies=usd&include_24hr_change=true&include_last_updated_at=true'
			);
			const data = await response.json();

			currentPrice = data.ergo.usd;
			priceChangePercent = data.ergo.usd_24h_change;
			priceChange = (currentPrice * priceChangePercent) / 100;
			lastUpdate = new Date(data.ergo.last_updated_at * 1000).toLocaleTimeString();
		} catch (error) {
			console.error('Error fetching current price:', error);
		}
	}

	async function fetchHistoricalData() {
		try {
			// Fetch 90 days of data (maximum for free tier)
			const response = await fetch(
				'https://api.coingecko.com/api/v3/coins/ergo/ohlc?vs_currency=usd&days=90'
			);
			const data = await response.json();

			candles = data.map((d: number[]) => ({
				time: d[0] / 1000,
				open: d[1],
				high: d[2],
				low: d[3],
				close: d[4]
			}));

			if (candleSeries) {
				candleSeries.setData(candles);
			}
		} catch (error) {
			console.error('Error fetching historical data:', error);
		}
	}

	onMount(async () => {
		// Initialize the chart
		chart = createChart(chartContainer, {
			width: chartContainer.clientWidth,
			height: 400,
			layout: {
				background: { type: ColorType.Solid, color: '#181818' },
				textColor: '#d1d1d1'
			},
			grid: {
				vertLines: { color: '#2b2b2b' },
				horzLines: { color: '#2b2b2b' }
			},
			rightPriceScale: {
				borderColor: '#2b2b2b',
				scaleMargins: {
					top: 0.1,
					bottom: 0.1
				}
			},
			timeScale: {
				borderColor: '#2b2b2b',
				timeVisible: true,
				secondsVisible: false
			},
			crosshair: {
				vertLine: {
					color: '#555',
					width: 1,
					style: 2,
					labelBackgroundColor: '#181818'
				},
				horzLine: {
					color: '#555',
					width: 1,
					style: 2,
					labelBackgroundColor: '#181818'
				}
			}
		});

		// Add candlestick series
		candleSeries = chart.addCandlestickSeries({
			upColor: '#4caf50',
			downColor: '#f44336',
			borderUpColor: '#4caf50',
			borderDownColor: '#f44336',
			wickUpColor: '#4caf50',
			wickDownColor: '#f44336'
		});

		// Fetch initial data
		await Promise.all([fetchCurrentPrice(), fetchHistoricalData()]);

		// Update price every 2 minutes (to stay within rate limits)
		updateInterval = setInterval(fetchCurrentPrice, 120000);

		// Resize chart on window resize
		const resizeObserver = new ResizeObserver(() => {
			chart.resize(chartContainer.clientWidth, 400);
		});
		resizeObserver.observe(chartContainer);

		return () => {
			resizeObserver.disconnect();
			chart.remove();
		};
	});

	onDestroy(() => {
		if (updateInterval) {
			clearInterval(updateInterval);
		}
	});
</script>

<div class="widget">
	<div class="header">
		<div class="pair">ERG/USD</div>
		<div class="prices">
			<div>
				Current Price <span class="price">${currentPrice.toFixed(4)}</span>
			</div>
			<div>
				24h Change <span class="pnl" class:pnl-negative={priceChange < 0}>
					${priceChange.toFixed(4)} (${priceChangePercent.toFixed(2)}%)
				</span>
			</div>
			<div class="last-update">
				Last updated: {lastUpdate}
			</div>
		</div>
	</div>

	<div bind:this={chartContainer} class="chart" />
</div>

<style>
	.widget {
		background-color: #181818;
		color: #d1d1d1;
		padding: 1rem;
		border-radius: 8px;
		font-family: Arial, sans-serif;
		max-width: 1200px;
		margin: auto;
	}
	.header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: 1rem;
	}
	.pair {
		font-size: 1.25rem;
		font-weight: bold;
	}
	.prices div {
		font-size: 0.9rem;
		margin-bottom: 0.2rem;
		text-align: right;
	}
	.price {
		font-weight: bold;
		margin-left: 0.5rem;
	}
	.pnl {
		font-weight: bold;
		margin-left: 0.5rem;
		color: #4caf50; /* Default color for positive */
	}
	.pnl-negative {
		color: #f44336; /* Color for negative PnL */
	}
	.chart {
		width: 100%;
		height: 400px;
	}
	.last-update {
		font-size: 0.8rem;
		color: #888;
	}
</style>
