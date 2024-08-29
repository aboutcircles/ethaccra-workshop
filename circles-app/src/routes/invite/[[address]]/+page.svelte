<script lang="ts">
    import {goto} from "$app/navigation";
    import {onMount} from "svelte";
    import {circlesAvatar} from "../../connect/+page.svelte"
    import {page} from "$app/stores";

    let address: string = $page.params.address ?? "";

    onMount(() => {
        if (!$circlesAvatar) {
            goto("/connect");
        }
    });

    async function invite() {
        await $circlesAvatar?.inviteHuman(address);
        await goto("/dashboard");
    }
</script>
<p>You can invite your friends to Circles. Specify the address of the wallet you want to invite.</p>
<label for="address">Address</label>
<input type="text" id="address" bind:value={address}/><br/>
<input type="button" value="Invite" on:click={invite}/>