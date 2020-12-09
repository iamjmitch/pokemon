<script>
	import { onMount } from 'svelte';
  	import { getPokemonList, getPokemonByName } from "./Services/API/Pokemon"; // import our pokemon api calls
	import AutoComplete from "simple-svelte-autocomplete";
	import PokemonDisplay from "./PokemonDisplay.svelte";
	import { toTitleCase } from "./Services/functions/AppFunctions"
	import getSprite from "./PokemonDisplay.svelte"

	let pokemonList = [], pokemon1, pokemon2;
	let pokeDisplay;
	
	onMount(async () => {
  		const res = await getPokemonList();
  		res.map((item) => (pokemonList = [...pokemonList, toTitleCase(item.name)]));
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
		display: flex;
	}
	.container{
		width: 50%;
		display:flex;
		flex-direction:column;
		align-items: center;
	}

</style>

<main>
	<div class="container left">
		{#if pokemonList.length > 0}
		<AutoComplete className="searchbar" items={pokemonList} bind:selectedItem={pokemon1} minCharactersToSearch=3  />
		{/if}
	</div>
	<div class="container left">
		{#if pokemonList.length > 0}
	<AutoComplete className="searchbar" items={pokemonList} bind:selectedItem={pokemon2} minCharactersToSearch=3 onChange="{() => pokeDisplay.getSprite()}" />
		<PokemonDisplay selectedPokemon={pokemon2} bind:this="{pokeDisplay}"/>
		{/if}
	</div>
</main>

