<script setup>

</script>

<template>
  <div class="bg-white rounded-lg mx-4 p-4 mt-8 space-y-4 text-Grayish-Blue">
    <textarea v-model="new_comment" placeholder="Add a comment..." class="border-2 rounded-lg h-28 w-full p-4"></textarea>
    
    <div class="flex justify-between items-center">
      <!-- <Avatar :avatar_path="current_user.image.png" /> -->
      <button @click="addComment" class="bg-Moderate-blue text-white py-2 px-6 rounded-lg">SEND</button>
    </div>
  </div>
</template>