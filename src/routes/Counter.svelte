<script>
	import poland from '$lib/images/poland.png';
	import europeanUnion from '$lib/images/european-union.png';
	import switzerland from '$lib/images/switzerland.png';
	let dropDownSale = false;
	let dropDownPurchase = false;
	let currencySale = 'choose currency';
	// @ts-ignore
	let prevCurrencySale;
	let currencyPurchase = 'choose currency';
	let currencesForSale = [
		{ id: 1, name: 'PLZ', img: poland, alt: '$lib/images/poland.png' },
		{ id: 2, name: 'CHF', img: europeanUnion, alt: 'europeanUnion' },
		{ id: 3, name: 'EUR', img: switzerland, alt: 'switzerland' },
	];
	let currencesForPurchase = [
		{ id: 4, name: 'PLZ', img: poland, alt: '$lib/images/poland.png' },
		{ id: 5, name: 'CHF', img: europeanUnion, alt: 'europeanUnion' },
		{ id: 6, name: 'EUR', img: switzerland, alt: 'switzerland' },
	];
	// @ts-ignore
	// @ts-ignore
	$: {
		// @ts-ignore
		if (prevCurrencySale && prevCurrencySale !== currencySale) {
			dropDownSale = false;
		}
		prevCurrencySale = currencySale;
	}
	const openDropDownSale = () => {
		dropDownSale = !dropDownSale;
	};
	const openDropDownPurchase = () => {
		dropDownPurchase = !dropDownPurchase;
	};
	// @ts-ignore
	const changeCurrencySale = (param) => {
		currencySale = param;
	};
	// @ts-ignore
	const changeCurrencyPurchase = (currency) => {
		currencyPurchase = currency;
	};
</script>

<div class="converter">
	<h1 class="converter__title">Currency Converter</h1>
	<p class="converter__text">
		Check live rates, set rate alerts, receive notifications and more.
	</p>
	<div class="converter__exchange">
		<div class="converter__exchange-item">
			<div class="converter__exchange-wrapper">
				<div>
					<button
						class="converter__exchange-currency"
						on:click={() => openDropDownSale()}
					>
						<span class="converter__exchange-currency__text"
							>{currencySale}</span
						>
						<div class="icon"></div>
					</button>
					{#if dropDownSale}
						<ul class="converter__exchange__dropdown">
							{#each currencesForSale as item (item.id)}
								<li class="converter__exchange__dropdown-item">
									<button
										on:click={() =>
											changeCurrencySale(item.name)}
										class="converter__exchange__dropdown-button"
									>
										<img
											src={item.img}
											alt={item.name}
											height="30"
											width="30"
										/>
										<span>{item.name}</span>
									</button>
								</li>
							{/each}
						</ul>
					{/if}
				</div>
				<div class="converter__exchange-info">
					<label class="converter__exchange-labels" for="amountPut">
						Amount
					</label>
					<input
						id="amountPut"
						name="amountPut"
						type="text"
						class="converter__exchange-input"
						placeholder="1000.00"
					/>
				</div>
			</div>
			<div class="converter__line"></div>
			<button class="converter__button"></button>
			<div class="converter__exchange-wrapper">
				<div>
					<button
						class="converter__exchange-currency"
						on:click={() => openDropDownPurchase()}
					>
						<span class="converter__exchange-currency__text"
							>{currencyPurchase}</span
						>
						<div class="icon"></div>
					</button>
					{#if dropDownPurchase}
						<ul class="converter__exchange-dropdown">
							{#each currencesForPurchase as item (item.id)}
								<li class="converter__exchange__dropdown-item">
									<button
										on:click={() =>
											changeCurrencyPurchase(item.name)}
										class="converter__exchange__dropdown-button"
									>
										<img
											src={item.img}
											alt={item.name}
											height="30"
											width="30"
										/>
										<span>{item.name}</span>
									</button>
								</li>
							{/each}
						</ul>
					{/if}
				</div>
				<div class="converter__exchange-info">
					<label class="converter__exchange-labels" for="amountGet">
						Amount
					</label>
					<input
						id="amountGet"
						name="amountGet"
						type="text"
						class="converter__exchange-input"
						placeholder="1000.00"
					/>
				</div>
			</div>
		</div>
	</div>
</div>

<style lang="scss">
	@import '/src/scss/reset.scss';
	.converter {
		font-family: 'Roboto', sans-serif;
	}
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
		padding: 20px;
		box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
		border-radius: 10px;
		position: relative;
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
		width: 133px;
		height: 45px;
		display: flex;
		align-items: center;
		justify-content: space-between;
		cursor: pointer;
		border: 1px solid #26278d;
		margin-bottom: 5px;
		border-radius: 5px;
		padding: 7px;
	}
	.converter__exchange__dropdown {
		margin-bottom: -5px;
	}
	.converter__exchange__dropdown-item {
		background: transparent;
		width: 133px;
		height: 45px;
		cursor: pointer;
		box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
		padding: 5px;
		margin-bottom: 5px;
		border-radius: 5px;
		border: 1px solid transparent;
	}
	.converter__exchange__dropdown-item:hover {
		border: 1px solid #26278d;
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
		min-height: 200px;
	}
	.converter__exchange-wrapper:first-child {
		margin-bottom: 32px;
	}
	.converter__exchange-info {
		display: flex;
		flex-direction: column;
	}
	.converter__button {
		background-color: #26278d;
		width: 44px;
		height: 44px;
		background-image: url('/src/lib/images/exchange.svg');
		background-position: center;
		background-repeat: no-repeat;
		border-radius: 50%;
		border: none;
		cursor: pointer;
		position: relative;
		z-index: 100;
		left: 50%;
		transform: translate(-50%, -50%);
	}
	.converter__line {
		height: 2px;
		background: #e7e7ee;
	}
	.converter__exchange-currency__text {
		max-width: 98px;
		min-width: 98px;
	}
	.converter__exchange__dropdown-button {
		background: transparent;
		cursor: pointer;
		border: none;
		padding-right: 38px;
		color: #26278d;
		font-weight: 700;
		font-size: 14px;
		display: flex;
		align-items: center;
		justify-content: space-between;
		width: 100%;
	}
</style>
