<script setup>
import ReplyIcon from "./icons/ReplyIcon.vue";
const props = defineProps(['comment', ])

</script>


<template>
  <div class="bg-white rounded-lg mx-4 p-4 mt-4 space-y-4 text-Grayish-Blue">
    <div class="space-x-3">
      <img :src="`/src/${comment.user.image.png}`" alt="avatar" class="h-8 w-8 inline">
      <span class="font-bold text-Dark-blue">{{comment.user.username}}</span>
      <span> {{comment.createdAt}} </span>
    </div>

    <p class="text-[16px] ">
      {{ comment.content }}
    </p>

    <div class="text-Moderate-blue flex justify-between">
      <div class="space-x-3 bg-Very-light-gray py-2 px-3 rounded-lg">
        <button class="text-Light-grayish-blue ">+</button>
        <span class="font-medium">{{comment.score}}</span>
        <button class="text-Light-grayish-blue">-</button>
      </div>

      <button class="font-bold flex items-center space-x-2">
        <i> <ReplyIcon /> </i>
        <span>Reply</span>
      </button>
    </div>
  </div>
</template>


<style lang="scss" scoped>

</style>