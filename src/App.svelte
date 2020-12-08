<script>
	import { onMount } from 'svelte';
  	import { getPokemonList, getPokemonByName } from "./Services/API/Pokemon"; // import our pokemon api calls

  	let pokemonDetail = {};
	let pokemonList = [];
	
	onMount(async () => {
    const res = await getPokemonList();
	pokemonList = res;
	console.log(pokemonList[0].name)
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


</style>

<main>
	{#if pokemonList.length > 0}
	{#each pokemonList as pokemon}
	<p>{pokemon.name}</p>
	{/each}
	{/if}
</main>

