<script>
    import {createEventDispatcher} from 'svelte';
    const disp = createEventDispatcher();
    let clicked = false;
    function stateDisp(state){
        disp('stateChanged',{state:state});
    }
    export let show;
</script>


<style>
    .open{
        position: absolute;
        left:0;
        width: 45vw;
        background-color: rgba(9, 33, 34, 0.842);
        height: 88vh;
        margin-left: 0;
        animation: 300ms slide;
    }
    @keyframes slide{
        from{left:-70%;}
        to{left:0%;}
    }
    .reg{
        margin-right:3vw;
        display: flex;
        justify-content: center;
    }
</style>

{#if show}
<div class={(clicked)?"reg open":"reg "}>
    <ul style="list-style: none; margin:0px; padding:0px;">
        <li>
            <div on:click={()=>clicked ^= true}>
            <span class="material-icons" style="margin-top:40px;font-size: xx-large;padding-top: 10px;">menu</span>
            </div>
        </li>
 
        <li>
            <div on:click={()=>stateDisp(2)}>
            <span class="material-icons" style="margin-top:40px;padding-top: 10px;" >chat</span>
            {#if clicked}
                Chat 
            {/if}
            </div>
        </li>

       <li>
            <div on:click={()=>stateDisp(1)}>
            <span class="material-icons" style="margin-top:40px;padding-top: 10px;" >group</span>
            {#if clicked}
                Groups
            {/if}
            </div>
        </li>
        <li>
            <div on:click={()=>stateDisp(3)}>
            <span class="material-icons"style="margin-top:40px;padding-top: 10px;">info</span>
            {#if clicked}
                Group Info
            {/if}
            </div>
        </li>
    </ul>
</div>
{/if}