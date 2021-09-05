<script>
	import { onMount } from 'svelte';
	import PlayerCard from './PlayerCard.svelte';
	import Loader from './Loader.svelte';

	let players = [];
	let name = '';
	let loading = true;

	onMount(async () => {
		const res = await fetch(`https://fessehaye.api.stdlib.com/gsheets-absmash@dev/`);
		players = await res.json();
		loading = false;
	});

	$: playersFiltered = players.filter(player => player.fields.Tags.toLowerCase().includes(name.toLowerCase()));
</script>

<main>
	
	<input bind:value={name} placeholder="Enter GamerTag">

	{#if loading}
		<Loader />
	{:else}
		<div class="playerList">
			{#each playersFiltered as player (player.index)}
				<PlayerCard player={player}  />
			{/each}
		</div>
	{/if}
	
	
</main>

<style>
	main {
		padding: 40px 10px;
	}

	input {
		background: transparent;
		border: none;
		border-bottom: 2px solid #fff;
		width: 100%;
		margin: 0 auto;
		margin-bottom: 50px;
		color: #fff;
		font-family: "Roboto", sans-serif;
		font-size: 18px;
		max-width: 900px;
		display: block;
	}

	:focus-visible {
		outline: none;
	}

	::placeholder { /* Chrome, Firefox, Opera, Safari 10.1+ */
		color: #fff;
		opacity: 1; /* Firefox */
		font-family: "Roboto", sans-serif;
	}

	@media only screen and (min-width: 620px) {
        .playerList {
            display: flex;
			flex-wrap: wrap;
			gap: 20px;
			justify-content: center;
        }

    }

</style>