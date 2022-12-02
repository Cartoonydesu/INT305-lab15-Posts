<script setup>
import CommentItem from "../components/CommentItem.vue"
import NewComment from "../components/NewComment.vue"
import { doc, collection, getDocs, query, onSnapshot, getCountFromServer } from "firebase/firestore"
import db from "../firebase/init.js"
import { ref } from "vue"

const props = defineProps({
  post: {
    type: Object,
    required: true,
  }
});

const countComment = ref(0)
const comments = ref([])

console.log(props.post)
// console.log(props.post.)

// const commentRef = collection(props.post, )

const cmtRef = collection(db, "posts", props.post.id, "comments")
onSnapshot(cmtRef, async () => {
  comments.value = []
  countComment.value = 0
  const cmtQuerySnap = await getDocs(query(cmtRef))
  cmtQuerySnap.forEach((doc) => {
    let data = doc.data()
    data.id = doc.id
    comments.value.push(data)
    countComment.value++
  })
  // const cmtCountQuery = await getCountFromServer(cmtRef)
  // countComment.value = cmtCountQuery.data().count
})



// const commentRef = collection(post, "posts", doc.id, "comments")




</script>

<template>
  <div class="box">
    {{post.body}}
    <!-- <h4 class="title">comments ({{post.comments.length}})</h4> -->
    <h4 class="title">comments ({{ countComment }})</h4>
    <NewComment :postId="post.id"/>
    <CommentItem v-for="comment in comments" :comment="comment" :key="comment.id" />
  </div>
</template>

<style scoped>
  .box {
    border: 1px solid grey;
    padding: 5px ;
    margin:  5px ;
    border-radius: 5px ;
  }
  .title {
    font-style: italic ;
    color: hsla(160, 100%, 37%, 1);
  }
</style>


