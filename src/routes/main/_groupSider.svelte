<script>
    export let data = [];
    export let showB = true;
</script>
<style>
    .container{
        padding-left: 10px;
        margin: 1vh 2vw 1vh 2vw;
    }
    .border{
        border-left:solid 0.5px rgb(5, 53, 53);
    }
    .member{
        background-color: rgba(5, 33, 46, 0.596);
        height: 5vh;
        padding-top: 2vh;
        padding-left: 1vw;
        font-size: large;
        border-radius: 5px;
        border-bottom: solid 0.5px rgba(30,100,100,0.3);
        margin-top:10px

    }
</style>

<div class={(showB)?"container border":"container"} style="height:80vh">
    <h1 style="font-weight: 200; font-size:30px;">Members</h1>
    <hr>
    {#each data as item}
        <div class="member">
            {item}
        </div> 
    {/each}

</div>