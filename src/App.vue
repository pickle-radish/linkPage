<template>
  <div style="width: 100px; margin:auto">
    <ul>
      <li>
        <a href="#" @click="clickCount(1)">linkpage1</a>
      </li>
      <li>
        <a href="#" @click="clickCount(2)">linkpage2</a>
      </li>
    </ul>
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
    clickCount(listNumber) {
      firebase.firestore().collection('url_list').where('listNumber', '==', listNumber).get()
          .then(snapshot => {
            console.log("snapshot");
            console.log(snapshot);
            if(!snapshot.empty){
              firebase.firestore().collection('url_list').doc(snapshot.docs[0].id)
              .update({
                count: snapshot.docs[0].data().count + 1
              })
            } else {
              firebase.firestore().collection('url_list')
              .add({
                listNumber,
                count: 1,
              })
            }
          })
          .then(
            alert("저장 되었습니다")
          )
          .catch(err=>{
            console.log("firebase error")
            console.log(err)
          })
    }
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
