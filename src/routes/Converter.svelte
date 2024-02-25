<script lang="js">
	// @ts-nocheck
	import { onMount } from 'svelte';
	import poland from '$lib/images/poland.png';
	import europeanUnion from '$lib/images/european-union.png';
	import switzerland from '$lib/images/switzerland.png';
	let dropDownSale = false;
	let dropDownPurchase = false;
	let currencySale = '';
	let currentCourses = null;
	let currencyPurchase = '';
	let mainCurrency = 'EUR';
	let trigger = false;
	/**
	 * @type {number}
	 */
	let sumForPut = '';
	// @ts-ignore
	let sumForGet = '';
	const currencesForSale = [
		{ id: 1, name: 'PLN', img: poland, alt: 'Poland' },
		{ id: 2, name: 'EUR', img: europeanUnion, alt: 'European Union' },
		{ id: 3, name: 'CHF', img: switzerland, alt: 'Switzerland' },
	];
	const currencesForPurchase = [
		{ id: 4, name: 'PLN', img: poland, alt: 'Poland' },
		{ id: 5, name: 'EUR', img: europeanUnion, alt: 'European Union' },
		{ id: 6, name: 'CHF', img: switzerland, alt: 'Switzerland' },
	];
	function load() {
		const url =
			'https://currency-converter-pro1.p.rapidapi.com/latest-rates?base=EUR';
		const options = {
			method: 'GET',
			headers: {
				'X-RapidAPI-Key':
					'36f9cbb627msha6f92731e797aeap104677jsn1653ead11c41',
				'X-RapidAPI-Host': 'currency-converter-pro1.p.rapidapi.com',
			},
		};
		fetch(url, options)
			.then((response) => response.json())
			.then((data) => {
				const { PLN, CHF, EUR } = data.result;
				currentCourses = { PLN, CHF, EUR };
			})
			.catch((error) => console.error(error));
	}
	function openDropDownSale() {
		dropDownSale = !dropDownSale;
	}
	function openDropDownPurchase() {
		dropDownPurchase = !dropDownPurchase;
	}
	function changeTrigger() {
		trigger = true;
	}
	function changeTriggerInput() {
		trigger = false;
	}
	/**
	 * @param {string} param
	 */
	function changeCurrencySale(param) {
		currencySale = param;
		dropDownSale = !dropDownSale;
	}
	/**
	 * @param {string} currency
	 */
	function changeCurrencyPurchase(currency) {
		currencyPurchase = currency;
		dropDownPurchase = !dropDownPurchase;
	}

	function changeCurrencySaleToPurchase() {
		if (currencySale && currencyPurchase) {
			const tempCurrency = currencySale;
			currencySale = currencyPurchase;
			currencyPurchase = tempCurrency;
			const tempSunGet = sumForGet;
			sumForPut = tempSunGet;
			if (currencySale === mainCurrency) {
				sumForGet = (
					sumForPut * currentCourses[currencyPurchase]
				).toFixed(2);
			}
			if (currencySale !== mainCurrency) {
				sumForGet = (
					sumForPut *
					(currentCourses[currencyPurchase] /
						currentCourses[currencySale])
				).toFixed(2);
			}
			if (currencyPurchase === mainCurrency) {
				sumForGet = (
					sumForPut *
					(1 / Number(currentCourses[currencySale]))
				).toFixed(2);
			}

			changeTrigger();
		}
	}
	function validateInput(event) {
		const input = event.target;
		const inputValue = input.value;
		const regex = /^\d*\.?\d*$/;
		if (!regex.test(inputValue)) {
			input.value = inputValue.replace(/[^\d.]/g, '');
		}
	}
	function handlerSumForGet(event) {
		if (currencySale && currencyPurchase) {
			if (currencySale === mainCurrency) {
				sumForGet = (
					event.currentTarget.value * currentCourses[currencyPurchase]
				).toFixed(2);
			}
			if (currencySale !== mainCurrency) {
				sumForGet = (
					event.currentTarget.value *
					(currentCourses[currencyPurchase] /
						currentCourses[currencySale])
				).toFixed(2);
			}
			if (currencyPurchase === mainCurrency) {
				sumForGet = (
					event.currentTarget.value *
					(1 / Number(currentCourses[currencySale]))
				).toFixed(2);
			}
		}
	}
	function handlerSumForPut(event) {
		if (currencySale && currencyPurchase) {
			if (currencySale === mainCurrency) {
				sumForPut = (
					event.currentTarget.value / currentCourses[currencyPurchase]
				).toFixed(2);
			}
			if (currencySale !== mainCurrency) {
				sumForPut = (
					event.currentTarget.value /
					(currentCourses[currencyPurchase] /
						currentCourses[currencySale])
				).toFixed(2);
			}
			if (currencyPurchase === mainCurrency) {
				sumForPut = (
					event.currentTarget.value /
					(1 / Number(currentCourses[currencySale]))
				).toFixed(2);
			}
		}
	}
	onMount(() => {
		load();
	});
	$: {
		if ((currencySale || currencyPurchase) && !trigger) {
			sumForGet = 0;
			sumForPut = 0;
		}
	}
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
						{#if currencySale === ''}
							<span class="converter__exchange-currency__text"
								>choose currency</span
							>
						{:else}
							<span class="converter__exchange-currency__text"
								>{currencySale}</span
							>
						{/if}
						<div class="icon"></div>
					</button>
					{#if dropDownSale}
						<ul class="converter__exchange__dropdown">
							{#each currencesForSale as item (item.id)}
								<li class="converter__exchange__dropdown-item">
									<button
										on:click={() => {
											changeCurrencySale(item.name);
											changeTriggerInput();
										}}
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
						type="text"
						name="amountPut"
						class="converter__exchange-input"
						placeholder="0"
						on:input={(event) => {
							validateInput(event);
							handlerSumForGet(event);
						}}
						bind:value={sumForPut}
					/>
				</div>
			</div>
			<div class="converter__line"></div>
			<button
				class="converter__button"
				on:click={() => changeCurrencySaleToPurchase()}
			></button>
			<div class="converter__exchange-wrapper">
				<div>
					<button
						class="converter__exchange-currency"
						on:click={() => openDropDownPurchase()}
					>
						{#if currencyPurchase === ''}
							<span class="converter__exchange-currency__text"
								>choose currency</span
							>
						{:else}
							<span class="converter__exchange-currency__text"
								>{currencyPurchase}</span
							>
						{/if}
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
						type="text"
						name="amountGet"
						class="converter__exchange-input"
						placeholder="0"
						on:input={(event) => {
							validateInput(event);
							handlerSumForPut(event);
						}}
						bind:value={sumForGet}
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
		border-radius: 5px;
		padding: 10px;
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
	.converter__exchange__dropdown-item:first-child {
		margin-top: 5px;
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
		-webkit-transition: background-color 1000ms linear;
		-ms-transition: background-color 1000ms linear;
		transition: background-color 1000ms linear;
	}
	.converter__button:hover,
	.converter__button:focus,
	.converter__button:active {
		background-color: black;
	}
	.converter__line {
		height: 2px;
		background: #e7e7ee;
	}
	.converter__exchange-currency__text {
		max-width: 90px;
		min-width: 90px;
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
