<script setup>
import { computed } from "@vue/reactivity";
import ReplyIcon from "./icons/ReplyIcon.vue";
import current_user from '@/composables/current_user.js'
import RDEButton from "./RDEButton.vue";
import DeleteIcon from "./icons/DeleteIcon.vue";
import EditIcon from "./icons/EditIcon.vue";
import Avatar from "./Avatar.vue";

const props = defineProps(['comment', ])

const ownership = computed(()=>{
  return props.comment.user.username == current_user.username
})

</script>


<template>
  <div class="bg-white rounded-lg mx-4 p-4 mt-4 space-y-4 text-Grayish-Blue">
    <div class="space-x-2">
      <Avatar :avatar_path="comment.user.image.png" />
      <span class="font-bold text-Dark-blue">{{comment.user.username}}</span>
      <span v-if="ownership" class="bg-Moderate-blue py-[2px] px-1 rounded-sm text-white text-xs">you</span>
      <span class="pl-2"> {{comment.createdAt}} </span>
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

      <div class="space-x-4 flex">
        <RDEButton v-if="!ownership" @click="$emit('showForm')">
          <template #icon>
            <ReplyIcon />
          </template>
          Reply
        </RDEButton>
  
        <RDEButton v-if="ownership" class="text-Soft-Red">
          <template #icon>
            <DeleteIcon />
          </template>
          Delete
        </RDEButton>
  
        <RDEButton v-if="ownership">
          <template #icon>
            <EditIcon />
          </template>
          Edit
        </RDEButton>
      </div>
    </div>
  </div>
</template>


<style lang="scss" scoped>

</style>