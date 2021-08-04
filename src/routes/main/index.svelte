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
    import { onDestroy, tick } from 'svelte';
    import { goto } from '$app/navigation';
import Sidebar from './_sidebar.svelte';
    const firebaseConfig = {
        apiKey: "AIzaSyAzuKsT78lSON8_qXfqCP6tmhnMlhXSDRQ",
        authDomain: "first-768e3.firebaseapp.com",
        projectId: "first-768e3",
        storageBucket: "first-768e3.appspot.com",
        messagingSenderId: "465550997622",
        appId: "1:465550997622:web:4851f28d45667630ad0757"
    } 

    let userName;
    let Uid;
    let state = 1;
    let sorry = false;
    let groups = [];
    let unsubscribe;
    let currentgname = {name:"Title",id:""};
    let innerWidth;
    let text;
    let arr = [];
    let sti;
    let memberlist = [];


    if(!firebase.apps.length)
        firebase.initializeApp(firebaseConfig);
    else
        firebase.app();

    const db  = firebase.firestore();

    function setGroup(){
        db.doc(`usr/${Uid}`).get()
        .then(dat=>{
            groups = dat.data().groups;
        })
        .catch(console.error);
    }

    firebase.auth().onAuthStateChanged(user=>{
        if(user == null){
            goto('/');
            sorry= true;
        }
        else {
            userName = (user.displayName)?user.displayName : user.email;
            Uid = user.uid;
            db.doc(`usr/${Uid}`).get()
            .then(dat=>{
                if(dat.exists){
                    const data = dat.data();
                    groups = data.groups;
                }
                else{
                    db.doc(`usr/${Uid}`).set({
                        username:userName,
                        groups:[]
                    })
                    .catch(console.error);
                }
            })
            .catch(console.error);
        }
    });

    onDestroy(uns);
 
    function signOut(){
        uns();
        firebase.auth().signOut()
        .then(succ=>{
        })
        .catch(err=>{
            console.error(err);
        });
    }

       
    function createHandler(event){
        console.log("create");
        const creator = db.collection('chat')
        creator.add({
                perm:[Uid],
                unames:[userName]
        })
        .then(dat=>{
            console.log("chat room created with id ", dat.id);
            creator.doc(dat.id).collection('chat-col').doc('Head').set({
                name : event.detail.name,
                unames: [userName]
            })
            .catch(console.error);
            db.doc(`usr/${Uid}`).update({groups:firebase.firestore.FieldValue.arrayUnion({name: event.detail.name, id:dat.id})})
            .then(setGroup)
            .catch(console.error);
        })
        .catch(console.error)
    }

    function joinHandler(event){
        const joiner = db.collection('chat').doc(event.detail.name);
        joiner.collection('chat-col').doc('Head').get()
        .then(dat=>{
            if(dat.exists){
                let gname = dat.data().name;
                //first
                joiner.update({perm:firebase.firestore.FieldValue.arrayUnion(Uid),
                    unames:firebase.firestore.FieldValue.arrayUnion(userName)})
                .then(dat=>{
                    console.log("Success 1");
                })
                .catch(console.error);
                //third
                joiner.collection('chat-col').doc('Head').update({
                    unames:firebase.firestore.FieldValue.arrayUnion(userName)
                })
                .catch(console.error)
                db.collection('usr').doc(Uid).update({groups:firebase.firestore.FieldValue.arrayUnion({name: gname, id: event.detail.name })})
                .then(dat=>{
                    console.log("Success 3");
                    setGroup();
                })
                .catch(console.error);

            }
        })
        .catch(console.error)
    }


    function groupSelect(event){
        uns();
        currentgname = event.detail;
        state = 2;
        unsubscribe = db.collection(`chat/${currentgname.id}/chat-col`).orderBy("time").limit(25).onSnapshot(async snapshot=>{
            arr = [];
            snapshot.forEach(doc=>{
                if(doc.id != 'Head')
                    arr.push(doc.data());
            });
            arr = [{name:'comp',message:`To add People to the group share this code ${currentgname.id}`},...arr];
            await tick();
        });
        db.doc(`chat/${currentgname.id}/chat-col/Head`).get()
        .then(async dat =>{
            memberlist = dat.data().unames;
            await tick();
            sti(arr.length-1);
        })
        .catch(console.error);

    }

    function uns(){
        if(currentgname.id != "")
            unsubscribe();
    }

    function beforeunload(){
        uns();
    }

    function stateChange(event){
        if(event.detail.state != state){
            state = event.detail.state;
        }
    }

    async function func(){
        // arr = [...arr,{name: `item${count}`, count: count}];
        // count++;
        // await tick();
        // await tick();
        // sti(arr.length-1);
    }
    function keyb(event){
        if(event.keyCode == 13)
            send();
    }
    function send(){
        if(currentgname.id){
            db.collection(`chat/${currentgname.id}/chat-col`).add({
                uid:Uid,
                time:firebase.firestore.FieldValue.serverTimestamp(),
                name:userName,
                message:text
            })
            .then(async dat=>{
                arr = [...arr,{message: text, name:userName}];
                await tick();
                await tick();
                sti(arr.length-1);
                text="";
            })
            .catch(console.error);
        }
        else{

        }
    }

</script>

<style>
    .yellow{
        background-color: rgba(90, 90, 90, 0.932);
        display: inline-flex;
        border-radius: 5px; 
        padding:10px;
        margin : 5px 20px 10px 20px;
        font-size: larger;
    }
    .right{
        float:right;
    }
    .bubble{
        background-color:rgb(126, 15, 177); 
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
            width: 80vw;
        }
        .bc{
            padding:0;
        }
    }
</style>

<svelte:window bind:innerWidth on:beforeunload={beforeunload}/>
{#if sorry}
<Nli/>
{:else}

    <div class={innerWidth >= 720? "cont":"smallcont"}>
        <div>
            <Sidebar show={innerWidth <= 720} on:stateChanged={stateChange}/>
        </div>
        <div>
        <Right showB={innerWidth > 720} listarr={groups} hidden={innerWidth <= 720 && state != 1} on:join={joinHandler}  on:create={createHandler} on:signout={signOut} on:group={groupSelect}/>
        </div>
        {#if innerWidth > 720 || state == 2}
        <div class="bc" >
            <h1 style="font-weight: 200; font-size:30px">
                {currentgname.name}
                <hr style="margin-bottom: 1vh;">
            </h1>
            <div class="scroll" id="box">
                <VirtualList bind:scrollToIndex={sti} items={arr} let:item>
                    <div class={(item.name == userName)? "right bubble" :(item.name == "comp")? "yellow":"bubble"} style="">
                        <span style="font-size: small;color:#ccc">
                            {item.name}
                        </span>
                        <br>
                        {item.message}
                    </div>
                </VirtualList>
            </div>
            <div style="display: flex;">
                <input type="text" class="bord" style="margin: 20px 10px 10px 0" on:keypress={keyb} bind:value={text}> 
                <div class="button" style="width:100px;" on:click={send}>Send</div>
            </div>
        </div>
        {/if}
        {#if  state == 3 || innerWidth > 720}
            <div>
                <GroupSider showB={innerWidth > 720} data={memberlist} />
            </div>
        {/if}
    </div>
{/if}
