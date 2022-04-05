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
  data.value = comments
  localStorage.setItem('comments', JSON.stringify(comments))
} else {
  data.value = JSON.parse(localStorage.getItem('comments'))
}

const addComment = (new_comment) => {
  const date = new Date()
  if (new_comment) {
    const current_comment = {
      id: Math.floor(Date.now() * Math.random()),
      content: new_comment,
      user: current_user,
      createdAt: [date.getFullYear(), date.getMonth(), date.getDate(), date.getHours(), date.getMinutes(), date.getSeconds()],
      score: 0,
      replies: [],
    }

    data.value.push(current_comment)
    localStorage.setItem('comments', JSON.stringify(data.value))
  }
}

const deleteComment = (id) => {
  // data.value = data.value.filter(comment => comment.id != id)
  const indexOfObject = data.value.findIndex(comment => {
    return comment.id === id;
  })
  data.value.splice(indexOfObject, 1)
  localStorage.setItem('comments', JSON.stringify(data.value))
}

</script>


<template>
  <!-- Pull dynamic content from the data.json file -->
  <div class="max-w-2xl mx-auto mt-10 md:mt-20 ">
    <template v-for="comment in data" :key="comment.id">
      <Comment :comment="comment" @delete="deleteComment" />
      <div v-if="comment.replies" class="border-l-2 ml-4 md:ml-8 md:pl-8">
        <template v-for="reply in comment.replies" :key="reply.id">
          <Comment :comment="reply" :parent="comment" />
        </template>
      </div>
    </template>
  
    <FormField @action="addComment" :place_holder="'Add a comment...'">
      <template #avatar>
        <Avatar :avatar_path="current_user.image.png" />
      </template>
      Send
    </FormField>
  </div>

  <footer class="mt-10 mb-2 text-center text-xs text-Grayish-Blue ">
    Challenge by <a href="https://www.frontendmentor.io?ref=challenge" target="_blank" class="underline hover:font-bold">Frontend Mentor</a>. 
      Coded by <a href="https://github.com/GuyTito/interactive-comments-section"  target="_blank" class="underline hover:font-bold">GuyTito</a>.
  </footer>
</template>


<style>

</style>
