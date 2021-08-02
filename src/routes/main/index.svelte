<script>
    //firebase
    import firebase from '@firebase/app';
    import '@firebase/firestore';
    import "@firebase/auth";

    //list
    import VirtualList from './_VList.svelte';

    //Svelte Components
    import Right from './_rightc.svelte';
    import Nli from './_nli.svelte';
    import GroupSider from './_groupSider.svelte';

    //svelte
    import { tick } from 'svelte';
import { goto } from '$app/navigation';
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

    let sorry = false;
    let uid = "";

    if(firebase.auth().currentUser == null){
        sorry = true;
    }
    else{
        uid = firebase.auth().currentUser.uid;
    } // To add closure just push this to the end
    sorry = false; // To remove before production

    const db  = firebase.firestore();

    //const usr = db.collection('usr').doc(`${uid}`);
    // usr.get()
    // .then(dat=>{
    //     if(dat.exists){
    //         console.log("It exists");
    //         console.log(dat.data());
    //     }
    //     else{
    //         usr.set({Message:"Welcome to the app"});
    //     }
    // })
    // .catch(err=>{
    //     console.log(err)
    // });

    function signOut(){
        firebase.auth().signOut()
        .then(succ=>{
            goto('/');
        })
        .catch(err=>{
            console.error(err);
        });

    }
        
    function createHandler(){
        console.log("create");

    }
    function joinHandler(event){
        console.log("join",event.detail.name);
        const joiner = db.collection('chat').doc(event.detail.name);
        joiner.get()
        .then(dat=>{
            if(dat.exists){
                let gname = dat.data().name;
                //first
                joiner.update({perm:firebase.firestore.FieldValue.arrayUnion(uid)})
                .then(dat=>{
                    console.log("Success 1");
                })
                .catch(console.error);
                
                //second
                joiner.update({name:firebase.firestore.FieldValue.arrayUnion(userName)});
                db.collection('usr').doc(uid)
                .then(dat=>{
                    console.log("Success 2");
                })
                .catch(console.error);

                //third
                db.collection('usr').doc(uid).update({groups:firebase.firestore.FieldValue.arrayUnion({name: gname, id: event.detail.name })})
                .then(dat=>{
                    console.log("Success 3")
                })
                .catch(console.error);

            }
        })
    }

    let arr = [];
    let count = 1;
    let sti;

    async function func(){
        arr = [...arr,{name: `item${count}`, count: count}];
        count++;
        await tick();
        await tick();
        sti(arr.length-1);
    }
    let innerWidth;
</script>

<style>
    .right{
        float:right;
    }
    .bubble{
        background-color:darkviolet; 
        border-radius: 5px; 
        display: inline-block;
        padding:10px;
        margin : 5px 20px 10px 20px;
        font-size: larger;
    
    }
    .scroll{
        height:100%; 
        border-radius:5px;
        background-color: #131e29;
        box-shadow: 2px 2px #0f171f;
        width:min(50vw,700px);
    }
    .bc{
        height:60vh;
        padding-left:50px;
    }
    @media screen and (max-width:720px){
        .scroll{
            background-color: aqua;
            width: 80vw;
        }
        .bc{
            padding:0;
        }
    }
</style>

{#if sorry}
<Nli/>
{/if}

<svelte:window bind:innerWidth/>
<div class="cont">
    <div>
    <Right hidden={innerWidth <= 720} on:join={joinHandler}  on:create={createHandler} on:signout={signOut}/>
    </div>
    <div class="bc" >
        <h1 style="font-weight: 200; font-size:30px">
            Title
            <hr style="margin-bottom: 1vh;">
        </h1>
        <div class="scroll" id="box">
             <VirtualList bind:scrollToIndex={sti} items={arr} let:item>
                <div class={(item.count%3 == 0)? "right bubble":"bubble"} style="">
                    <span style="font-size: small;color:#ccc">
                        Name <!--Name to add-->
                    </span>
                    <br>
                    {item.name}
                </div>
            </VirtualList>
        </div>
        <div style="display: flex;">
            <input type="text" class="bord" style="margin: 20px 10px 10px 0"> 
            <div class="button" style="width:100px;" >Send</div>
        </div>
        <button on:click={func}>Click here</button>
   </div>
   {#if innerWidth > 720}
   <div>
       <GroupSider />
   </div>
   {/if}
</div>

