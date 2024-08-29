<script lang="ts">
    import {page} from "$app/stores";
    import {circlesAvatar} from "../../connect/+page.svelte"
    import {onMount} from "svelte";
    import {goto} from "$app/navigation";

    let to: string = $page.params.to ?? "";
    let totalCirclesBalance: number = 0;
    let amount: number = 0;
    let maxTransferableAmount: bigint | undefined = 0n;

    onMount(async () => {
        if (!$circlesAvatar) {
            await goto("/connect");
            return;
        }

        totalCirclesBalance = await $circlesAvatar?.getTotalBalance() ?? 0;
    });

    async function send() {
        await $circlesAvatar?.transfer(to, amount);
        await goto("/dashboard");
    }

    $: {
        $circlesAvatar?.getMaxTransferableAmount(to)
            .then((value) => maxTransferableAmount = value / 1000000000000000000n)
            .catch(() => maxTransferableAmount = 0n);
    }
</script>
<p>You have {totalCirclesBalance.toFixed(2)} Circles that can potentially be sent to another Circles user. <br/>
    The actual transferable amount depends on your trust network and the receiver.</p>

<p>Send <input type="number" bind:value={amount}/> Circles to <input type="text" bind:value={to}/></p>
<p><i>Max transferable value is: {maxTransferableAmount}</i></p>
<button on:click={send} disabled={maxTransferableAmount === 0n}>Send</button>