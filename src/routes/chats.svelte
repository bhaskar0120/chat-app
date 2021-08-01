<script>
    import List from "./main/_list.svelte";
    export let hidden = false;
    let gname = "";

</script>

{#if hidden}
<span class="material-icons" style="color:#f0f0f0; font-size: xx-large;">menu</span>
{:else}
<div style="border-right: solid 1px rgba(30,100,100,0.5);margin:2vh 2vw 1vh 2vw;font-size:large; height:80vh;">
    <input type="text" placeholder="Chat" class="bord" name="joing" bind:value={gname}>
    <List/>
</div>
{/if}