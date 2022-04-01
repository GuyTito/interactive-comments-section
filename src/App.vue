<script setup>
import { ref } from 'vue';
import comments from '@/composables/comments.js'
import current_user from "@/composables/current_user.js";

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
  <div >
    <div v-for="comment in data" :key="comment.id">
      <p class="mb-3">{{ comment.content }}</p>
      <template v-if="comment.replies">
        <div v-for="reply in comment.replies" :key="reply.id" class="ml-4 mb-3">
          {{ reply.content }}
        </div>
      </template>
    </div>
  </div>

  <div>
    <textarea v-model="new_comment" placeholder="Add a comment..." class="border-2"></textarea>
    <button @click="addComment">Send</button>
  </div>

  <!-- <footer class="mt-10 mb-2 text-center text-xs text-Dark-gray">
    Challenge by <a href="https://www.frontendmentor.io?ref=challenge" target="_blank" class="underline hover:font-bold">Frontend Mentor</a>. 
      Coded by <a href="https://github.com/GuyTito/interactive-comments-section"  target="_blank" class="underline hover:font-bold">GuyTito</a>.
  </footer> -->
</template>


<style>

</style>
