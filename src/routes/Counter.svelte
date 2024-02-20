<script>
	import { spring } from 'svelte/motion';
	import poland from '$lib/images/poland.png';
	import europeanUnion from '$lib/images/european-union.png';
	import euro from '$lib/images/european-union.png';
	import switzerland from '$lib/images/switzerland.png';
	let count = 0;

	const displayed_count = spring();
	$: displayed_count.set(count);
	$: offset = modulo($displayed_count, 1);

	/**
	 * @param {number} n
	 * @param {number} m
	 */
	function modulo(n, m) {
		// handle negative numbers
		return ((n % m) + m) % m;
	}
</script>

<div class="converter">
	<h1 class="converter__title">Currency Converter</h1>
	<p class="converter__text">
		Check live rates, set rate alerts, receive notifications and more.
	</p>
	<ul class="converter__exchange">
		<li class="converter__exchange-item">
			<div class="converter__exchange-wrapper">
				<div>
					<button class="converter__exchange-currency">
						<span>choose currency</span>
						<div class="icon"></div>
					</button>
					<ul class="converter__exchange-dropdown">
						<li class="converter__exchange-dropdown-item">
							<img
								src={poland}
								alt="poland"
								height="30"
								width="30"
							/>
							<span>PLZ</span>
						</li>
						<li class="converter__exchange-dropdown-item">
							<img
								src={switzerland}
								alt="switzerland"
								height="30"
								width="30"
							/>
							<span>CHF</span>
						</li>
						<li class="converter__exchange-dropdown-item">
							<img
								src={europeanUnion}
								alt="europeanUnion"
								height="30"
								width="30"
							/>
							<span>EUR</span>
						</li>
					</ul>
				</div>
				<div class="converter__exchange-info">
					<label class="converter__exchange-labels" for="amount">
						Amount
					</label>
					<input
						id="amount"
						name="amount"
						type="text"
						class="converter__exchange-input"
						placeholder="1000.00"
					/>
				</div>
			</div>
		</li>
	</ul>
	<!-- <button on:click={() => (count -= 1)} aria-label="Decrease the counter by one">
		<svg aria-hidden="true" viewBox="0 0 1 1">
			<path d="M0,0.5 L1,0.5" />
		</svg>
	</button>

	<div class="counter-viewport">
		<div class="counter-digits" style="transform: translate(0, {100 * offset}%)">
			<strong class="hidden" aria-hidden="true">{Math.floor($displayed_count + 1)}</strong>
			<strong>{Math.floor($displayed_count)}</strong>
		</div>
	</div>

	<button on:click={() => (count += 1)} aria-label="Increase the counter by one">
		<svg aria-hidden="true" viewBox="0 0 1 1">
			<path d="M0,0.5 L1,0.5 M0.5,0 L0.5,1" />
		</svg>
	</button> -->
</div>

<style lang="scss">
	@import '/src/scss/base.scss';
	@import '/src/scss/reset.scss';
	.converter__title {
		color: #1f2261;
		font-weight: 700;
		font-size: 35px;
		margin-bottom: 16px;
	}
	.converter__text {
		color: #808080;
		font-size: 16px;
		font-weight: 400;
		margin-bottom: 32px;
	}
	.converter__exchange {
		background: #ffffff;
		padding: 10px;
		box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
		border-radius: 10px;
	}
	.converter__exchange-labels {
		color: #989898;
		font-size: 12px;
		margin-bottom: 10px;
	}
	.converter__exchange-input {
		background: #efefef;
		padding-bottom: 5px;
		width: 141px;
		height: 35px;
		padding: 5px;
		border: none;
		border-radius: 5px;
		color: #3c3c3c;
		font-size: 12px;
		outline: none;
	}
	.converter__exchange-currency {
		background: transparent;
		border: none;
		color: #26278d;
		font-weight: 700;
		font-size: 14px;
		width: 123px;
		height: 45px;
		display: flex;
		align-items: center;
		justify-content: space-between;
		cursor: pointer;
	}
	.converter__exchange-dropdown-item {
		background: transparent;
		border: none;
		color: #26278d;
		font-weight: 700;
		font-size: 14px;
		width: 123px;
		height: 45px;
		display: flex;
		align-items: center;
		justify-content: space-between;
		cursor: pointer;
		box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
		padding: 5px;
		padding-right: 38px;
		border: 5px;
		margin-bottom: 5px;
	}
	.icon {
		width: 11px;
		height: 7px;
		background-image: url('/src/lib/images/arrow.svg');
		background-size: cover;
		background-repeat: no-repeat;
	}
	.converter__exchange-wrapper {
		display: flex;
		justify-content: space-between;
		margin-bottom: 32px;
	}
	.converter__exchange-info {
		display: flex;
		flex-direction: column;
		padding-top: 10px;
	}
</style>
