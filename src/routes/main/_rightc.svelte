<script>
    import { element } from "svelte/internal";
import List from "./_list.svelte";
    export let hidden = false;
</script>

{#if hidden}
<span class="material-icons" style="color:#f0f0f0; font-size: xx-large;">menu</span>
{:else}
<div style="margin:5px;font-size:large; border-right: solid 1px rgba(30,100,100,0.5);height:90vh;">
    <input type="text" placeholder="Chat" class="bord">
    <List/>
</div>
{/if}