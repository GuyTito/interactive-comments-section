<script setup>
import { ref } from 'vue';
import comments from '@/composables/comments.js'
import current_user from "@/composables/current_user.js";
import Comment from "./components/Comment.vue";
import Avatar from './components/Avatar.vue';
import FormField from './components/FormField.vue';

const data = ref([])

// check for comments in localstorage and retrieve
if(!localStorage.getItem('comments')) {
  localStorage.setItem('comments', JSON.stringify(comments))
} else {
  data.value = JSON.parse(localStorage.getItem('comments'))
}

const addComment = (new_comment) => {
  if (new_comment) {
    const current_comment = {
      id: Math.floor(Date.now() * Math.random()),
      content: new_comment,
      user: current_user,
      createdAt: Date.now(),
      score: 0,
      replies: [],
    }

    data.value.push(current_comment)
    localStorage.setItem('comments', JSON.stringify(data.value))
  }
}

</script>


<template>
  <!-- Pull dynamic content from the data.json file -->
  <template v-for="comment in data" :key="comment.id">
    <Comment :comment="comment" />
    <div v-if="comment.replies" class="border-l-2 ml-4">
      <template v-for="reply in comment.replies" :key="reply.id">
        <Comment :comment="reply" :parent="comment" />
      </template>
    </div>
  </template>

  <FormField @send="addComment" :place_holder="'Add a comment...'">
    <template #avatar>
      <Avatar :avatar_path="current_user.image.png" />
    </template>
    Send
  </FormField>

  <footer class="mt-10 mb-2 text-center text-xs text-Grayish-Blue ">
    Challenge by <a href="https://www.frontendmentor.io?ref=challenge" target="_blank" class="underline hover:font-bold">Frontend Mentor</a>. 
      Coded by <a href="https://github.com/GuyTito/interactive-comments-section"  target="_blank" class="underline hover:font-bold">GuyTito</a>.
  </footer>
</template>


<style>

</style>
