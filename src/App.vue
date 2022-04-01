<script setup>
import { ref } from 'vue';
import comments from '@/composables/data.js'

const data = ref(null)
const currentComment = ref('')
// check for comments in localstorage and retrieve
if(!localStorage.getItem('comments')) {
  localStorage.setItem('comments', JSON.stringify(comments))
} else {
  data.value = JSON.parse(localStorage.getItem('comments'))
}

</script>


<template>
    <!-- Pull dynamic content from the data.json file -->
  <div >
    <div v-for="comment in comments" :key="comment.id">
      <p class="mb-3">{{ comment.content }}</p>
      <template v-if="comment.replies">
        <div v-for="reply in comment.replies" :key="reply.id" class="ml-4 mb-3">
          {{ reply.content }}
        </div>
      </template>
    </div>
  </div>

  <div>
    <textarea v-model="currentComment" placeholder="Add a comment..." class="border-2"></textarea>
    <button @click="send">Send</button>
  </div>

  <!-- <footer class="mt-10 mb-2 text-center text-xs text-Dark-gray">
    Challenge by <a href="https://www.frontendmentor.io?ref=challenge" target="_blank" class="underline hover:font-bold">Frontend Mentor</a>. 
      Coded by <a href="https://github.com/GuyTito/interactive-comments-section"  target="_blank" class="underline hover:font-bold">GuyTito</a>.
  </footer> -->
</template>


<style>

</style>
