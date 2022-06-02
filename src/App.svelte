<script>
	import {Table} from 'sveltestrap'
	import { Styles } from 'sveltestrap';
	import {onMount} from "svelte";
	let coins = [];
	let filteredCoins = [];
	let titles = [
		'#',
		'Coin',
		'Price',
		'Price Change',
		'Volume for 24hs'
	]
	let ref = null;
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

	onMount(()=>{
		ref.focus();
	})
</script>

<svelte:head>
	  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css">
</svelte:head>

<main>
	<div class="container">
		<div class="row">
			<h1>CryptoFinder</h1>
			<input type="text" class="form-control bg-dark text-white rounded-0 border-0 my-4" 
			placeholder="Search your coin" 
			on:keyup={({target: {value}}) => searchCoin(value)}
			bind:this={ref}>
			<Table bordered dark striped responsive>
				<thead>
					<tr>
						{#each titles as title }
							<th>{title}</th>
						{/each}
					</tr>
				</thead>
				<tbody>
					{#each filteredCoins as coin, i}	  
						<tr>
							<td style="color:darkgray;">{i+1}</td>
							<td>
								<span>
									<img src={coin.image} alt={coin.name} style="width: 2rem;" class="img-fluid me-2">
									{coin.name}
								</span>
								<span class="text-uppercase ms-2" style="color:dimgrey;">
									{coin.symbol}
								</span>
							</td>
							<td>
								${coin.current_price.toLocaleString()}
							</td>
							<td class={coin.price_change_percentage_24h>0 ? "text-success" : "text-danger"}>
								{coin.price_change_percentage_24h} %
							</td>
							<td>
								$ {coin.total_volume.toLocaleString()}
							</td>
						</tr>
					{/each}
					
				</tbody>
			</Table>
		</div>
	</div>

</main>

<style>
	
</style>