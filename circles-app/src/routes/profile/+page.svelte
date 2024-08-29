<script lang="ts">
    import {onMount} from "svelte";
    import {goto} from "$app/navigation";
    import {circlesAvatar} from "../connect/+page.svelte"

    let name: string = "";
    let hasGas: boolean = false;

    onMount(async () => {
        if (!$circlesAvatar) {
            await goto("/connect");
            return;
        }

        const circlesProfile = await $circlesAvatar.getProfile();
        name = circlesProfile?.name ?? "";

        const gas = await $circlesAvatar.getGasTokenBalance();
        hasGas = gas > 0;
    });

    async function saveProfile() {
        if (!$circlesAvatar) {
            return;
        }

        await $circlesAvatar.updateProfile({name});
        await goto("/");
    }
</script>
<p>Create a new profile so that others can recognize you.</p>
<div>
    <label for="name">Name</label>
    <input type="text" id="name" bind:value={name}/><br/>
    <input type="button" value="Save" disabled={!hasGas} on:click={saveProfile}/>
</div>
{#if !hasGas}
    <p><i style="color: red">Not enough gas.</i></p>
    <i>Send some xDai to {$circlesAvatar?.address}, then reload the page</i>
{/if}