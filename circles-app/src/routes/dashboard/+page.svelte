<script lang="ts">
    import {onMount} from "svelte";
    import {goto} from "$app/navigation";
    import {circlesAddress, circlesAvatar, circlesSdk} from "../connect/+page.svelte"
    import type {Profile} from "@circles-sdk/profiles";
    import type {TokenBalanceRow, TrustRelationRow} from "@circles-sdk/data";

    let profile: Profile | undefined;
    let balance: number | undefined;
    let mintableAmount: number | undefined;
    let contacts: TrustRelationRow[] = [];
    let individualCircles: TokenBalanceRow[] | undefined = [];

    onMount(async () => {
        if (!$circlesAvatar) {
            await goto("/connect");
            return;
        }
        if (!$circlesAvatar.avatarInfo?.cidV0) {
            await goto("/profile");
            return;
        }

        [profile, balance, mintableAmount, contacts, individualCircles] = await Promise.all([
            $circlesAvatar.getProfile(),
            $circlesAvatar.getTotalBalance(),
            $circlesAvatar.getMintableAmount(),
            $circlesAvatar.getTrustRelations(),
            $circlesSdk?.data.getTokenBalancesV2($circlesAddress!)
        ]);
    });
</script>
<h1>Hello {profile?.name ?? ""} <button on:click={() => goto("/profile")}>Edit name</button></h1>
<p>Your Circles address is {$circlesAddress ?? ""}</p>

<h2>You have {balance?.toFixed(2) ?? "0.00"} Circles</h2>
<p>... and can currently <button on:click={() => goto("/mint")}>mint</button> {mintableAmount?.toFixed(2) ?? "0.00"} new personal Circles</p>
{#if (balance ?? 0) > 0}
    <i>You hold the following individual Circles that, in total, make up your balance:</i>
    <ul>
        {#each (individualCircles ?? []) as individualCircle}
            <li>{individualCircle.tokenOwner}: {individualCircle.balance}</li>
        {/each}
    </ul>
{/if}

<h2>You have {contacts.length} contacts</h2>
    <p>
        You can <button on:click={() => goto("/invite")}>invite</button> others to Circles, or
        <button on:click={() => goto("/trust/add")}>trust</button> someone who has already joined.
    </p>
{#if contacts.length > 0}
    Your avatar currently
    <ul>
        {#each contacts as contact}
            <li>
                <p>... {contact.relation}: {contact.objectAvatar}</p>
                {#if contact.relation === "mutuallyTrusts"}
                    <button on:click={() => goto("/send/" + contact.objectAvatar)}>Send Circles</button>
                    <button on:click={() => goto("/trust/remove/" + contact.objectAvatar)}>Untrust</button>
                {:else if contact.relation === "trusts"}
                    <button on:click={() => goto("/trust/remove/" + contact.objectAvatar)}>Untrust</button>
                {:else if contact.relation === "trustedBy"}
                    <button on:click={() => goto("/trust/add/" + contact.objectAvatar)}>Trust</button>
                {/if}
            </li>
        {/each}
    </ul>
{/if}