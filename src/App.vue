<template>
  <div>
    <a href="http://www.naver.com" @click="clickCount(1)">naver.com</a>
  </div>  
</template>

<script>

export default {
  name: 'App',
  components: {
    // HelloWorld
  },

  methods: {
    clickCount(listNumber) {
      firebase.firestore().collection('url_list').where('listNumber', '==', listNumber).get()
          .then(snapshot => {
            if(!snapshot.empty){
              firebase.firestore().collection('url_list').doc(snapshot.id)
              .update({
                count: snapshot.data().count + 1
              })
            } else {
              firebase.firestore().collection('url_list').doc(snapshot.id)
              .add({
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
