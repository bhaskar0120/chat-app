<script>
    import {createEventDispatcher} from 'svelte';
    export let highl = false;
    export let index;
    export let name = "Bhaskar"
    const dip= createEventDispatcher();
    function onClick(){
        //highl ^= true;
        dip('changed', {val:highl,index:index});

    }

</script>

<style>
    .high{
        background-image: linear-gradient(to right, rgba(170,170,255,0.1),rgba(255,255,255,0));
        border-radius: 5px;
    }
    div{
        height: 50px;
        padding-top:15px;
        border-bottom: solid 0.5px rgba(27, 53, 53, 0.774);
        width:80%;
        font-size: large;
        font-weight: 200;
        margin:20px;
    }
</style>

<div  class:high="{highl}" on:click={onClick}>
    {name}
</div>