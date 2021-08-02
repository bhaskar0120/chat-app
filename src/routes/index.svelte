<script>
    import firebase from '@firebase/app';
    import "@firebase/auth";
    import {goto} from '$app/navigation';
    const firebaseConfig = {
        apiKey: "AIzaSyAzuKsT78lSON8_qXfqCP6tmhnMlhXSDRQ",
        authDomain: "first-768e3.firebaseapp.com",
        projectId: "first-768e3",
        storageBucket: "first-768e3.appspot.com",
        messagingSenderId: "465550997622",
        appId: "1:465550997622:web:4851f28d45667630ad0757"
    } 
    if(!firebase.apps.length)
        firebase.initializeApp(firebaseConfig);
    else
        firebase.app();
    
    let email, password, err = "";
    
    function errorFunc(e){
        err = e.message;
    }
    function gmail(){
        firebase.auth().signInWithPopup(new firebase.auth.GoogleAuthProvider)
        .then(succ=>{
            goto('/main');
        })
        .catch(errorFunc);
    }
    function onClick(){
        if(email && password){
            firebase.auth().signInWithEmailAndPassword(email,password)
            .then(userC=>{
               goto('/main');
            })
            .catch(errorFunc);
        }
        else errorFunc({message:"No email or password provided"});
        
    }

</script>

<center>
    <div style="font-size: 1000%; color:#66fcf1;font-family:-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;">
        Hello.
    </div>
    <span style="font-size: x-large;">
        A very exclusive messaging app.
    </span>

</center>
<br> <br> <br>

    <div >
            <div style=" font-weight: 400; font-size: 300%;height:30vh; width:30vh;margin:auto; padding:10px">

                Login<br>
                <input type="email" placeholder="Email" bind:value={email} style="height:3vh;width:30vh;border-radius:3px;border:solid 0.5px gray">
                <input type="password" placeholder="Password" bind:value={password} style="height:3vh;width:30vh;border-radius:3px;border:solid 0.5px gray">
                <div class="button" on:click={onClick}>Login</div>
                <div class="button" on:click={gmail}>Login with Google</div>
                {#if err}
                <div style="padding:10px;margin-top:2vh;border-radius: 5px; background:rgba(255, 110, 110, 0.6); font-size: medium;">{err}</div>
                {/if}
            </div>
    
    </div>