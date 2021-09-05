<script>
	import { fade,fly } from 'svelte/transition';

    export let i;
    export let player;
</script>

<div class="playerCard" in:fly="{{ y: 200, duration: 1500, delay: i*50 }}" out:fade>
    <span class="playerName">{player.fields.tags}</span>
    <span class="playerEvent">{player.fields.event}</span>
</div>

<style>
    .playerCard {
        background: #fff;
        width: 500px; 
        box-sizing: border-box;
        display: flex;
        align-items: center;
        margin-bottom: 15px;
        padding: 15px 20px;
        justify-self: normal;
        border-radius: 5px;
        font-size: 18px;
        box-shadow: rgba(50, 50, 93, 0.25) 0px 50px 100px -20px, rgba(0, 0, 0, 0.3) 0px 30px 60px -30px, rgba(10, 37, 64, 0.35) 0px -2px 6px 0px inset;
    }

    @media only screen and (max-width: 500px) {
        .playerEvent,.playerName,.playerCard {
            display: block;
        }

        .playerCard {
            width: 100%;
        }


        .playerName {
            margin-bottom: 10px;
        }
    }

    @media only screen and (max-width: 1050px) {

        .playerCard {
            width: 100%;
        }

    }


    .playerName {
        font-weight: 700;
        margin-right: auto;
        color:#374151;
    }

    .playerEvent {
        color:#6B7280;
        font-size: 16px;
    }
</style>