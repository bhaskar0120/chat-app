<script>
    import List from "./_list.svelte";
    import {goto} from '$app/navigation';
    import {createEventDispatcher} from 'svelte';

    export let hidden = false;
    export let showB = true;
    export let listarr;
    let gname = "", newname="";
    let clicked = false;
    let count = listarr.length;
    for(let i = 0; i < count;++i){
        let x = listarr[i];
        x.glow=false;
    }

    const dispatch = createEventDispatcher();
    function joinHandler(){
        if(gname)
            dispatch('join',{name:gname});
    }
    function createHandler(){
        if(newname)
            dispatch('create',{name:newname});
    }
    
    function signOut(){
        dispatch('signout',{name:"logout"});
    }

    //Think of a better change function by remembering the last and current
    let selected = -1;
    function change(event){
        listarr[event.detail.index].glow ^= true;
        if(selected > -1)
            listarr[selected].glow = false;
        selected = listarr[event.detail.index].glow?event.detail.index:-1
        if(selected > -1)
            dispatch('group',{name:listarr[selected].name,id:listarr[selected].id})
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
    .borderS{
        border-right: solid 1px rgba(30,100,100,0.5);
        transition: 300ms;
    }
</style>

{#if hidden}
<div style="margin-right:3vw;display: flex; justify-content: center;" class={(clicked)?"open":""}>
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
<div class={(showB)?"borderS":""} style="margin:1vh 2vw 1vh 2vw;font-size:large; height:80vh;">
    <input type="text" placeholder="Enter Group Code" class="bord" bind:value={gname} style="font-size: medium; ">
    <div class={(gname)?"button":"gray"} style="width:12vw;font-size:large;text-align: center; margin: 0 0 0 .7vw" on:click={joinHandler}>Join Group</div>

    <input type="text" placeholder="New Group Name" class="bord" bind:value={newname} style="font-size: medium; ">
    <div class={(newname)?"button":"gray"} style="width:12vw;font-size:large;text-align: center; margin: 0 0 0 .7vw" on:click={createHandler}>Create Group</div>
    {#each listarr as item, i}
        <List on:changed={change} highl={item.glow} name={item.name} index={i}/>
    {/each}
    <div class="button"style="width:12vw;font-size:large;text-align: center; margin: 10vh 0 0 .7vw; " on:click={signOut}>Sign Out</div>
</div>
{/if}