<script lang="ts">
    import {goto} from "$app/navigation";
    import {onMount} from "svelte";
    import {circlesAvatar} from "../../../connect/+page.svelte"
    import {page} from "$app/stores";

    let action: "add" | "remove" = $page.params.action?.toLowerCase() !== "remove" ? "add" : "remove";
    let address: string = $page.params.address ?? "";

    onMount(() => {
        if (!$circlesAvatar) {
            goto("/connect");
        }
    });

    async function trustOrUntrust() {
        if (action === "add") {
            await $circlesAvatar?.trust(address);
        } else {
            await $circlesAvatar?.untrust(address);
        }
        await goto("/dashboard");
    }
</script>
{#if action === "add"}
    <p>Trusting someone means that you are willing to treat their Circles as equal to yours.</p>
{:else}
    <p>Untrusting someone means that you are no longer willing to treat their Circles as equal to yours.</p>
{/if}
<label for="address">Address</label>
<input type="text" id="address" bind:value={address}/><br/>
<input type="button" value={action === "remove" ? "Untrust" : "Trust" } on:click={trustOrUntrust}/>