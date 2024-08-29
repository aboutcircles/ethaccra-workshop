<script context="module" lang="ts">
    /*
       This is Svelte-specific syntax that separates module-level code (shared across all instances)
       from component-level code (specific to each instance). The first (<script context="module">) is
       for global imports and stores, while the second <script> is for local state and logic.
    */
    import {writable} from "svelte/store";
    import {type Avatar, Sdk} from '@circles-sdk/sdk';

    //
    // Step 1: Define the global stores for the SDK, the address and the avatar object.
    //
    export const circlesSdk = writable<Sdk | undefined>();
    export const circlesAddress = writable<string | undefined>();
    export const circlesAvatar = writable<Avatar | undefined>();
</script>
<script lang="ts">
    import {onDestroy, onMount} from "svelte";
    import {BrowserProviderContractRunner} from "@circles-sdk/adapter-ethers"
    import {config} from "$lib/config";
    import {goto} from "$app/navigation";

    let loading = true;
    let unsubscribe: () => void;

    onMount(async () => {
        //
        // Step 2: Initialize the SDK utilizing the browser wallet (window.ethereum).
        //         This step can only be done in onMount() because it requires the window object.
        //
        const adapter = new BrowserProviderContractRunner();
        await adapter.init();

        // Assign the address to the global store
        // (the '$' prefix is svelte specific syntax for global stores).
        $circlesAddress = adapter.address?.toLowerCase();

        // Assign the SDK to the global store
        $circlesSdk = new Sdk(config, adapter);

        //
        // Step 3: Check if the wallet is signed up at Circles.
        //         If it is, fetch the avatar info and assign it to the global store.
        //
        const avatarInfo = await $circlesSdk.data.getAvatarInfo($circlesAddress!);
        if (!avatarInfo || avatarInfo.version !== 2) {
            loading = false;

            // Wait for an invitation to be sent to the address.
            unsubscribe = await subscribeToInvitations();
            return;
        }

        // Assign the avatar to the global store.
        $circlesAvatar = await $circlesSdk.getAvatar($circlesAddress!);
        loading = false;

        // If the user has an avatar, redirect to the dashboard
        if ($circlesAvatar) {
            await next();
        } else {
            throw new Error(`Couldn't fetch avatar for address ${$circlesAddress}`);
        }
    });

    onDestroy(() => {
        if (unsubscribe) {
            unsubscribe();
        }
    });

    async function next() {
        await goto("/");
    }

    async function subscribeToInvitations() {
        const eventObservable = await $circlesSdk!.data.subscribeToEvents();
        return eventObservable.subscribe(async (event) => {
            if (event.$event !== "CrcV2_InviteHuman") {
                return;
            }
            if (event.invited !== $circlesAddress) {
                return;
            }

            await next();
        });
    }
</script>
{#if loading}
    <p>Loading...</p>
{:else}
    {#if !$circlesAvatar}
        <p>You need an invitation to join Circles.</p>
        <p>Ask someone with a Circles account to invite you and share this address with them:</p>
        <p>{$circlesAddress}</p>
    {:else}
        <p>You have a Circles account and should be redirected to the <a href="/profile">profile</a>.</p>
    {/if}
{/if}