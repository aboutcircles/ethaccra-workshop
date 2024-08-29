<script lang="ts">
    import {onMount} from "svelte";
    import {goto} from "$app/navigation";
    import {circlesAvatar} from "./connect/+page.svelte"

    onMount(() => {
        if (!$circlesAvatar) {
            goto("/connect");
        } else {
            goto("/dashboard");
        }
    });
</script>