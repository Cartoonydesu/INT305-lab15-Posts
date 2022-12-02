<script setup>
import { RouterLink, RouterView } from 'vue-router'

let props = defineProps({
  users: {
    type: Array,
    required: true
  }
})

  console.log(props.users)
</script>

<template>
  <div class="greetings">
    <h3>User List</h3>
    <ul>
        <li v-for="user in users" :key="user.id">
          <!-- <router-link :to="{ name: 'ThisEvent', params:{eventId:event.id}}"></router-link> -->
          <router-link :to="{name: 'posts', params:{userId: user.id}}">{{ user.firstname }}</router-link>
          
          <!-- <RouterLink to="{name: 'posts', params:{userId: user.id}}" >{{ user.firstname }}</RouterLink> -->
            <!-- <router-link :to="{name: 'posts', user: user.id}">{{ user.firstname }}</router-link> -->
        </li>
    </ul>
    
    <br><br>
  <p>หมายเหตุ อ่อน Frontend มากค่ะ พยายามทำทั้ง project เองเลยค่ะ แต่ทำ computed ไม่เป็น อย่างไรก็ตาม ได้คิด algorithm ให้ ข้อมูล update แบบ realtime แน่นอนค่ะ</p>
  
  </div>
</template>

<style scoped>

</style>
