<script lang="ts">
    import {goto} from "$app/navigation";
    import {onMount} from "svelte";
    import {circlesAvatar} from "../connect/+page.svelte"

    let mintableAmount: number = 0;

    onMount(async () => {
        if (!$circlesAvatar) {
            goto("/connect");
        }

        mintableAmount = await $circlesAvatar?.getMintableAmount() ?? 0;
    });

    async function mint() {
        await $circlesAvatar?.personalMint();
        await goto("/dashboard");
    }
</script>
<p>
    Every human can mint one Circle per hour unconditionally.
</p>
{#if mintableAmount > 0}
    <i>You can currently mint {mintableAmount.toFixed(2)} Circles</i>.
    <input type="button" value="Mint personal Circles" on:click={mint}/>
{:else}
    <i>You can't mint any Circles right now. Wait until one hour passed since your last minting.</i>
{/if}