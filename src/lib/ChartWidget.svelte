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

	let candles: CandlestickData[] = [];

	async function fetchCurrentPrice() {
		try {
			const response = await fetch(
				'https://api.codetabs.com/v1/proxy/?quest=' +
					encodeURIComponent(
						'https://api.kucoin.com/api/v1/market/orderbook/level1?symbol=ERG-USDT'
					)
			);
			const data = await response.json();

			currentPrice = parseFloat(data.data.price);

			const statsResponse = await fetch(
				'https://api.codetabs.com/v1/proxy/?quest=' +
					encodeURIComponent('https://api.kucoin.com/api/v1/market/stats?symbol=ERG-USDT')
			);
			const statsData = await statsResponse.json();

			const openPrice = parseFloat(statsData.data.open);
			priceChange = currentPrice - openPrice;
			priceChangePercent = (priceChange / openPrice) * 100;
			lastUpdate = new Date().toLocaleTimeString();
		} catch (error) {
			console.error('Error fetching current price:', error);
		}
	}

	async function fetchHistoricalData() {
		try {
			const endAt = Math.floor(Date.now() / 1000);
			const startAt = endAt - 90 * 24 * 60 * 60;

			const response = await fetch(
				'https://api.codetabs.com/v1/proxy/?quest=' +
					encodeURIComponent(
						`https://api.kucoin.com/api/v1/market/candles?symbol=ERG-USDT&type=1day&startAt=${startAt}&endAt=${endAt}`
					)
			);
			const data = await response.json();

			const candleData = data.data.reverse();

			candles = candleData.map((d: any[]) => ({
				time: parseInt(d[0]),
				open: parseFloat(d[1]),
				high: parseFloat(d[3]),
				low: parseFloat(d[4]),
				close: parseFloat(d[2])
			}));

			if (candleSeries) {
				candleSeries.setData(candles);
			}
		} catch (error) {
			console.error('Error fetching historical data:', error);
		}
	}

	onMount(async () => {
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

		candleSeries = chart.addCandlestickSeries({
			upColor: '#4caf50',
			downColor: '#f44336',
			borderUpColor: '#4caf50',
			borderDownColor: '#f44336',
			wickUpColor: '#4caf50',
			wickDownColor: '#f44336'
		});

		await Promise.all([fetchCurrentPrice(), fetchHistoricalData()]);

		updateInterval = setInterval(fetchCurrentPrice, 120000);

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
	<div class="header" style="width:399px;">
		<div class="pair">ERG/USDT</div>
		<!-- <div class="prices">
			<div>
				Current Price <span class="price">${currentPrice.toFixed(4)}</span>
			</div>
			<div class="last-update">
				Last updated: {lastUpdate}
			</div>
		</div> -->
	</div>

	<div bind:this={chartContainer} class="chart"></div>
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
		font-size: 1.15rem;
		font-weight: bold;
	}
	.chart {
		width: 100%;
		height: 100px;
	}
</style>
