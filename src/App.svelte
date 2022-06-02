<script>
	import {Table} from 'sveltestrap'
	import { Styles } from 'sveltestrap';
	import {
    Card,
    CardBody,
    CardSubtitle,
    CardTitle,
	Input
  } from 'sveltestrap';
	let coins = [];
	let filteredCoins = [];
	let loadCoins = async() => {
		let response = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false');
		coins =  await response.json();
		filteredCoins = coins;
	}
	loadCoins()
	let searchCoin = (value)=>{
		filteredCoins = coins.filter(coin => coin.name.toLowerCase().includes(value.toLowerCase()) ||
		coin.symbol.toLowerCase().includes(value.toLowerCase()))
	}

	
</script>

<svelte:head>
	  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css">
</svelte:head>

<main>
	<h1>CryptoFinder</h1>
	<div class="container-lg">
		<Input class="form-control bg-dark text-white rounded-0 border-0 my-4 "
		placeholder="Search your coin" 
		on:keyup={({target: {value}}) => searchCoin(value)} style="max-width:100%;">
	</Input>
	</div>
		<div class="container">
			{#each filteredCoins as coin, i}	  
				<Card style="width: 300px;" class="mb-3 bg-dark col" >
					<div class="g-0 text-center row">
						<div class="col-md-4"><br>
							<img src= {coin.image} alt={coin.name} style="padding-left:20px;" class="img-fluid rounded-start">
						</div>
						<div class="col-md-8">
							<CardBody>
								<CardTitle class="bs-light ">{coin.name}</CardTitle><br>
								<CardSubtitle>${coin.current_price.toLocaleString()}</CardSubtitle><br>
								<CardSubtitle class={coin.price_change_percentage_24h>0 ? "text-success" : "text-danger"}>{coin.price_change_percentage_24h} %</CardSubtitle>
							</CardBody>
						</div>
					</div>
				</Card>
			{/each}
	</div>
</main>

<style>
	
</style>