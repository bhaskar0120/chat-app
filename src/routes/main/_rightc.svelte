<script>
    import List from "./_list.svelte";
    export let hidden = false;
    let gname = "";
    function clickHandler(){
        //pass
    }
</script>

{#if hidden}
<span class="material-icons" style="color:#f0f0f0; font-size: xx-large;">menu</span>
{:else}
<div style="border-right: solid 1px rgba(30,100,100,0.5);margin:1vh 2vw 1vh 2vw;font-size:large; height:80vh;">
    <input type="text" placeholder="Chat" class="bord" bind:value={gname}>
    <div class={(gname)?"button":"gray"} style="width:90%;" on:click={clickHandler}>Join Group</div>
    <List/>
</div>
{/if}