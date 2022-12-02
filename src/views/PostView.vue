<script setup>
import { ref, onMounted, watch } from 'vue'
import { useRoute } from "vue-router"
import { collection, query, where, getDocs, onSnapshot, getDoc, doc, QuerySnapshot, getCountFromServer } from "firebase/firestore"
import db from "../firebase/init.js"
import PostItem from "../components/PostItem.vue"
import { computed } from '@vue/reactivity'

const user = ref({})
const posts = ref([])
const route = useRoute() 
const userId = ref("")
const countPost = ref(0)
// console.log("userId: ", userId.value)

async function getPosts(){
  userId.value = route.params.userId
  console.log(userId.value)

  const userRef = collection(db, "users");
  let userQry = query(userRef, where("__name__","==", userId.value));
  const userQuerySnap = await getDocs(userQry)

  userQuerySnap.forEach((doc) => {
    console.log("forEach")
    console.log(doc.data())
    let data = doc.data();
    data.id = doc.id;
    user.value = data;
    console.log(user.value)
  })

  console.log(user.value)

  const postRef = collection(db, "posts")
  const postQry = query(postRef, where("user", "==", userId.value))
  console.log("start on snapshot post")
  onSnapshot(postRef, async () => {
    posts.value = []
    countPost.value = 0
    const postQuerySnap = await getDocs(postQry);
    postQuerySnap.forEach((doc) => {
      let data = doc.data()
      data.id = doc.id
      posts.value.push(data)
      console.log(data)
      countPost.value++
    })
  })
  // const countPostQuery = await getCountFromServer(postQry)
  // countPost.value = countPostQuery.data().count

  

}

// computed = () => {
// const countPost = async computed => {
//       const postRef = collection(db, "posts")
//       const postQry = query(postRef, where("user", "==", userId.value))
//       const countPostQuery = await getCountFromServer(postQry)
//       // countPost.value = countPostQuery.data().count
//       return countPostQuery.data().count
// }
  // }

  // let countPost = new Vue({
  //   data: {
      
  //   },
  //   computed: {
  //     function(){return "qq"}
  //   }
  // })

watch( () => route.params.userId, getPosts)

onMounted(() => {
  getPosts() 
})

</script>

<template>
    <h3>Posts : {{ user.firstname }} {{ user.lastname }} ({{ countPost }})</h3>
    <PostItem v-for="post in posts" :post="post" :key="post.id" />
</template>

<style scoped>

</style>

