<script>
    import List from "./_list.svelte";
    import {goto} from '$app/navigation';
    export let hidden = false;
    let gname = "";
    let clicked = false;
    function clickHandler(){
        //pass
    }
</script>

<style>
    .open{
        position: absolute;
        left:0;
        width: 45vw;
        background-color: rgba(9, 33, 34, 0.842);
        height: 88vh;
        margin-left: 0;
    }
</style>

{#if hidden}
<div style="margin-right:3vw;" class={(clicked)?"open":""}>
    <ul style="list-style: none; margin:0px; padding:0px;">
        <li>
            <div on:click={()=>clicked ^= true}>
            <span class="material-icons" style="margin-top:40px;font-size: xx-large;padding-top: 10px;">menu</span>
            </div>
        </li>
        <li>
            <div on:click={()=>goto('/chats')}>
            <span class="material-icons" style="margin-top:40px;padding-top: 10px;" >group</span>
            {#if clicked}
                Chat Groups
            {/if}
            </div>
        </li>
        <li>
            <div on:click={()=>goto('/groupinfo')}>
            <span class="material-icons"style="margin-top:40px;padding-top: 10px;">info</span>
            {#if clicked}
                Group Info
            {/if}
            </div>
        </li>
    </ul>
</div>
{:else}
<div style="border-right: solid 1px rgba(30,100,100,0.5);margin:1vh 2vw 1vh 2vw;font-size:large; height:80vh;">
    <input type="text" placeholder="Chat" class="bord" bind:value={gname}>
    <div class={(gname)?"button":"gray"} style="width:90%;font-size:large;text-align: center;padding:2px 0 0 0;" on:click={clickHandler}>Join Group</div>
    <List/>
</div>
{/if}