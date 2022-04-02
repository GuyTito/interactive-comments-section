<script setup>
import { computed, ref } from "@vue/reactivity";
import ReplyIcon from "./icons/ReplyIcon.vue";
import current_user from '@/composables/current_user.js'
import RDEButton from "./RDEButton.vue";
import DeleteIcon from "./icons/DeleteIcon.vue";
import EditIcon from "./icons/EditIcon.vue";
import Avatar from "./Avatar.vue";
import FormField from "./FormField.vue";

const props = defineProps(['comment', ])

const ownership = computed(()=>{
  return props.comment.user.username == current_user.username
})

const show_form = ref(false)
const toggleForm = () => {
  show_form.value = !show_form.value
}

const data = ref([])

const addReply = (new_reply)=> {
  if (new_reply) {
    const current_reply = {
      id: Math.floor(Date.now() * Math.random()),
      content: new_reply,
      user: current_user,
      createdAt: Date.now(),
      score: 0,
      replyingTo: props.comment.user.username,
    }

      
    data.value = JSON.parse(localStorage.getItem('comments'))

    data.value.forEach(comment => {
      if (comment.id == props.comment.id) {
        props.comment.replies.push(current_reply)
        comment.replies.push(current_reply)
      }

      // detect a reply
    });

    localStorage.setItem('comments', JSON.stringify(data.value))
    show_form.value = false
  }
}

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
      <span v-if="comment.replyingTo" class="text-Moderate-blue font-bold">@{{comment.replyingTo}}</span> {{ comment.content }}
    </p>

    <div class="text-Moderate-blue flex justify-between">
      <div class="space-x-3 bg-Very-light-gray py-2 px-3 rounded-lg">
        <button class="text-Light-grayish-blue ">+</button>
        <span class="font-medium">{{comment.score}}</span>
        <button class="text-Light-grayish-blue">-</button>
      </div>

      <div class="space-x-4 flex">
        <RDEButton v-if="!ownership" @click="toggleForm">
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

  <FormField v-if="show_form" @send="addReply" :place_holder="'Reply...'">
    <template #avatar>
      <Avatar :avatar_path="current_user.image.png" />
    </template>
    Reply
  </FormField>
</template>


<style lang="scss" scoped>

</style>