<script>
	import { onMount } from 'svelte';
	import { fade } from 'svelte/transition';

	import PlayerCard from './PlayerCard.svelte';
	import Loader from './Loader.svelte';

	let players = [];
	let name = '';
	let loading = true;

	let timer;

	const debounce = v => {
		clearTimeout(timer);
		timer = setTimeout(() => {
			name = v;
		}, 500);
	}

	onMount(async () => {
		const res = await fetch(`https://fessehaye.api.stdlib.com/gsheets-absmash@dev/`);
		players = await res.json();
		console.log(players);
		loading = false;
	});

	$: playersFiltered = players.filter(player => player.fields.tags.toLowerCase().includes(name.toLowerCase()));
</script>

<main>
	<p class="count">
		{#if !loading}
			{playersFiltered.length} Players
		{/if}
	</p>
	<input on:keyup={({ target: { value } }) => debounce(value)} placeholder="Enter GamerTag">

	{#if loading}
		<Loader />
	{:else}
		{#if playersFiltered.length > 0}
			<div class="playerList">
				{#each playersFiltered as player,i (player.index)}
					<PlayerCard player={player} i={i} />
				{/each}
			</div>
		{:else}
			<p class="empty" transition:fade="{{ duration: 1000 }}">Player can not be found</p>
		{/if}
	{/if}
	
	
</main>

<style>
	main {
		padding: 40px 10px;
	}

	.count {
		color: #fff;
		font-family: "Roboto", sans-serif;
		font-size: 18px;
		max-width: 900px;
		margin: 0 auto;
		text-align: right;
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

	.empty {
		color:#fff;
		width: 100%;
		text-align: center;
		text-transform: uppercase;
		font-weight: 700;
		font-size: 24px;
	}

</style>