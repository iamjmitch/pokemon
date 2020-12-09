<script>
	import { onMount } from 'svelte';  	
	import AutoComplete from "simple-svelte-autocomplete";

    export let selectedPokemon = "";
    let spriteURL, spriteHeight = 0, brightness = 1;     

    export async function getSprite() {
        if (selectedPokemon !== undefined) {
            const data = await getPokemonByName(selectedPokemon.toLowerCase());
            preloadImage(`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${data.id}.png`, data.height
            );
        }
    }

    function preloadImage(url, height) {
        brightness = 0;
        var img = new Image();
        img.src = url;
        img.addEventListener('load', () => {
            spriteURL = url;
            spriteHeight = height;
            brightness = 1;
        });
    }   

</script>

<style>
	.pokeimg{
		position: absolute;
		bottom:100px;
		left: 50%;
		transform:translateX(-50%);
			}

    </style>

    <div class="pokeContainer">
	    {#if spriteURL !== undefined}
            <img style={`height:${spriteHeight*35}px;filter:brightness(${brightness}); max-height:90vh;`} class="pokeimg" src={spriteURL} alt={selectedPokemon}/>
	    {/if}
	</div>


