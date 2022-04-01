<script setup>import { ref } from 'vue';

const data = ref(null)
// check if we have comments in localstorage
if(!localStorage.getItem('comments')) {
  const comments = {boy: 'kofi', girl: 'ama'} 
  localStorage.setItem('comments', JSON.stringify(comments))
} else {
  data.value = JSON.parse(localStorage.getItem('comments'))

}

</script>


<template>
  <div class="text-Moderate-blue ">
    <!-- Pull dynamic content from the data.json file -->
    {{data.girl}}
  </div>

  <footer class="mt-10 mb-2 text-center text-xs text-Dark-gray">
    Challenge by <a href="https://www.frontendmentor.io?ref=challenge" target="_blank" class="underline hover:font-bold">Frontend Mentor</a>. 
      Coded by <a href="https://github.com/GuyTito/crowdfunding-product-page"  target="_blank" class="underline hover:font-bold">GuyTito</a>.
  </footer>
</template>


<style>

</style>
