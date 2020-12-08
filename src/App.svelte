<script>
	import { onMount } from 'svelte';
  	import { getPokemonList, getPokemonByName } from "./Services/API/Pokemon"; // import our pokemon api calls
	import AutoComplete from "simple-svelte-autocomplete";

  	let pokemonDetail = {};
	let pokemonList = [];
	let selectedPokemon;
	let spriteURL;
	let spriteHeight;

	function toTitleCase(str) {
 	 return str.replace(/\w\S*/g, function(txt) {return txt.charAt(0).toUpperCase() + txt.substr(1).toLowerCase();
    }
  );
}

async function getSprite(){
	if (selectedPokemon !== undefined){	
		const data = await getPokemonByName(selectedPokemon.toLowerCase());		
		console.log(data);
		preloadImage(`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${data.id}.png`, data.height);
		
}
}

function preloadImage(url, height)
{
    var img=new Image();
	img.src=url;
	img.addEventListener('load', () => { spriteURL = url;
		spriteHeight = height;})
	
	
}


	
	onMount(async () => {
	const res = await getPokemonList();

	res.map(item => pokemonList = [...pokemonList, toTitleCase(item.name)])
  });





</script>

<style>
	main {
		background: url(../assets/img/background.png);
		width: 100%;
		min-height: 100vh;
		background-attachment: fixed;
		background-position: bottom;
		background-size: cover;	
	}
	.pokeimg{
		position: absolute;
		bottom:100px;
		left: 50%;
		transform:translateX(-50%);
		
	}

	
	
</style>

<main>

	{#if pokemonList.length > 0}
	<AutoComplete className="searchbar" items={pokemonList} bind:selectedItem={selectedPokemon} minCharactersToSearch=3 onChange={getSprite} />
	{#if spriteURL !== undefined}
<img style={`height:${spriteHeight*40}px;`} class="pokeimg" src={spriteURL} alt={selectedPokemon}/>
	{/if}
	{/if}
</main>

