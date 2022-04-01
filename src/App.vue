<script setup>
import { ref } from 'vue';
import comments from '@/composables/comments.js'
import current_user from "@/composables/current_user.js";
import Comment from "./components/Comment.vue";

const data = ref([])
const new_comment = ref('')

// check for comments in localstorage and retrieve
if(!localStorage.getItem('comments')) {
  localStorage.setItem('comments', JSON.stringify(comments))
} else {
  data.value = JSON.parse(localStorage.getItem('comments'))
}

const addComment = () => {
  // write to check a reply or comment and code accordingly
  if (new_comment.value) {
    const current_comment = {
      id: Math.floor(Date.now() * Math.random()),
      content: new_comment.value,
      user: current_user,
      createdAt: Date.now(),
      score: 0,
      replies: [],
    }

    data.value.push(current_comment)
    localStorage.setItem('comments', JSON.stringify(data.value))

    new_comment.value = ''
  }
}

</script>


<template>
  <!-- Pull dynamic content from the data.json file -->
  <template v-for="comment in data" :key="comment.id">
    <Comment :comment="comment" />
    <div v-if="comment.replies" class="border-l-2 ml-4">
      <template v-for="reply in comment.replies" :key="reply.id">
        <Comment :comment="reply" />
      </template>
    </div>
  </template>

  <div class="bg-white rounded-lg mx-4 p-4 mt-8 space-y-4 text-Grayish-Blue">
    <textarea v-model="new_comment" placeholder="Add a comment..." class="border-2 rounded-lg h-28 w-full p-4"></textarea>
    
    <div class="flex justify-between items-center">
      <img :src="`/src/${current_user.image.png}`" alt="avatar" class="h-8 w-8 inline">
      <button @click="addComment" class="bg-Moderate-blue text-white py-2 px-6 rounded-lg">SEND</button>
    </div>
  </div>

  <!-- <footer class="mt-10 mb-2 text-center text-xs text-Dark-gray">
    Challenge by <a href="https://www.frontendmentor.io?ref=challenge" target="_blank" class="underline hover:font-bold">Frontend Mentor</a>. 
      Coded by <a href="https://github.com/GuyTito/interactive-comments-section"  target="_blank" class="underline hover:font-bold">GuyTito</a>.
  </footer> -->
</template>


<style>

</style>
