<script setup>
import { addDoc, collection } from "@firebase/firestore";
import {ref} from "vue"
import db from "../firebase/init.js"

const props = defineProps({
    postId: {
        type: String,
        required: true
    }
})

const name = ref("")
const stars = ref()
const comment = ref("")

const addComment = async function() {
    const docRef = collection(db, "posts", props.postId, "comments")
    addDoc(docRef, {
        name: name.value,
        stars: stars.value,
        comment : comment.value,
        cmtdate: new Date()
    })
    name.value = ""
    stars.value = null
    comment.value = null
}

</script>


<template>
    <div class="box">
        <h3>Create new comment</h3>
        Name : <input v-model="name" /><br/>
        Stars: <input type="number" v-model="stars" /><br/>
        Comment: <input v-model="comment" /><br/>
        <button @click="addComment">Send</button>
    </div>
</template>



<style>

</style>


