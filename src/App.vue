<template>
  <div>
    <div style="width: 100px; margin:auto">
      <ul>
        <li>
          <a href="#" @click="clickCount(1)">linkpage1</a>
        </li>
        <li>
          <a href="#" @click="clickCount(2)">linkpage2</a>
        </li>
        <li>
          <a href="#" @click="clickCount(3)">linkpage3</a>
        </li>
      </ul>
    </div>
    <div style="margin: auto; margin-top: 500px">
      <span>link count</span>
      <div style="width:200px; margin:auto">
        <ul>
          <li>
            <span>page1 link count : <span id="1-count">0</span></span>
          </li>
          <li>
            <span>page2 link count : <span id="2-count">0</span></span>
          </li>
          <li>
            <span>page3 link count : <span id="3-count">0</span></span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import firebase from '@/firebase/init'

export default {
  name: 'App',
  components: {
    // HelloWorld
  },

  methods: {
    async clickCount(listNumber) {
      try {
        let snapshot = await firebase.firestore().collection('url_list').where('listNumber', '==', listNumber).get();

        if(!snapshot.empty){
          await firebase.firestore().collection('url_list').doc(snapshot.docs[0].id)
          .update({
            count: snapshot.docs[0].data().count + 1
          })
        }

        let url = await snapshot.docs[0].data().url;

        location.href=url;

      } catch(err) {
        console.log("firebase err");
      }
    },

    loadCount() {
      firebase.firestore().collection('url_list').get()
        .then(snapshot => {
          snapshot.forEach(doc => {
            document.getElementById(doc.data().listNumber+"-count").textContent = doc.data().count;
          })
        })
    }
  },
  
  created(){
    this.loadCount();
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
