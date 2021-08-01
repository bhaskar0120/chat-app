<svelte:head>
    <link href="https://fonts.googleapis.com/icon?family=Material+Icons"
      rel="stylesheet">
</svelte:head>
<style>
    :global(body){
        background-color:#0c131a;
        color : white;
        margin:0 5% 0 5%;
    }
    @media screen and (max-width:580px){
        :global(body){
            margin: 0 10px 0 10px;
        }
    }
</style>
<nav >
    <center>
    <span class="material-icons" style="color:#66fcf1;margin-top:20px; font-size: xxx-large;">chat</span>
    <span style="font-size:xx-large; ">
        Messenger
    </span>
    </center>
</nav>
<hr>
<slot></slot>