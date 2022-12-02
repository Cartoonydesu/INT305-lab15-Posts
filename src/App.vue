<script setup>
import { RouterLink, RouterView } from 'vue-router'
import { ref, onMounted } from 'vue'
import { query, collection, getDocs, onSnapshot, doc, QuerySnapshot, getDoc } from 'firebase/firestore'
import db from './firebase/init.js'
import UserList from './components/UserList.vue'

const users = ref([])

async function getUsers(){
  const dbRef = collection(db, "users")
    // onSnapshot(dbRef, (snapshot)=>{
    //   users.value = []
    //   snapshot.forEach((doc) => {
    //     console.log(doc.data())
    //     const data = doc.data()
    //     users.value.push(data)
    //     console.log(data)
    //   })
    // })

    onSnapshot(dbRef,async ()=> {
      users.value = []
      console.log("On snap shot")
      const querySnap = await getDocs(query(dbRef));
      querySnap.forEach((doc) => {
        let data = doc.data()
        data.id = doc.id
        users.value.push(data)
        // console.log(data.id)
      })
    })

    // console.log("getUsers")
    // const querySnap = await getDocs(query(collection(db, 'users')));
    // users.value = []
    // querySnap.forEach((doc) => {
    //   let data = doc.data()
    //   data.id = doc.id
    //   users.value.push(data)
    //   console.log(data)
    //   // console.log(users)
    // })
}

onMounted(() => {
    getUsers() 
})
</script>

<template>
  <div>
    <div>
      <UserList :users="users" />
    </div>
  </div>
  <div class="content">
    <RouterView />
    
  </div>

</template>

<style scoped>

</style>

