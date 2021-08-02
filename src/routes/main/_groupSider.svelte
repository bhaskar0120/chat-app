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
</style>

<div class={(showB)?"container border":"container"} style="height:80vh">
    <h1 style="font-weight: 200; font-size:30px;">Members</h1>
    <hr>

</div>