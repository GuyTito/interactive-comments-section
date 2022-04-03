<script setup>
import { computed, ref } from "@vue/reactivity";
import ReplyIcon from "./icons/ReplyIcon.vue";
import current_user from '@/composables/current_user.js'
import RDEButton from "./RDEButton.vue";
import DeleteIcon from "./icons/DeleteIcon.vue";
import EditIcon from "./icons/EditIcon.vue";
import Avatar from "./Avatar.vue";
import FormField from "./FormField.vue";

const props = defineProps(['comment', 'parent', ])

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
    // reply to the right reply
    if (props.comment.hasOwnProperty('replyingTo')) {
      data.value.forEach(comment => {
        if (comment.id == props.parent.id) {
          props.parent.replies.push(current_reply)
          comment.replies.push(current_reply)
        }
      });
    }else{
      data.value.forEach(comment => {
        // reply to the right comment
        if (comment.id == props.comment.id) {
          props.comment.replies.push(current_reply)
          comment.replies.push(current_reply)
        }
      });
    }
    localStorage.setItem('comments', JSON.stringify(data.value))

    show_form.value = false
  }
}

const show_modal = ref(false)

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
  
        <RDEButton v-if="ownership" @click="show_modal = true" class="text-Soft-Red">
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

  <FormField v-if="show_form" @send="addReply" :place_holder="'Reply...'" :to="comment.user.username">
    <template #avatar>
      <Avatar :avatar_path="current_user.image.png" />
    </template>
    Reply
  </FormField>

  <Teleport to="body" v-if="show_modal">
    <div class="fixed inset-0 bg-black/40 grid place-content-center">
      <div class="bg-white rounded-lg p-7 mx-12 text-Grayish-Blue space-y-5">
        <h3 class="font-bold">Delete comment</h3>
        <p>Are you sure you want to delete this comment? This will remove the comment and can't be undone.</p>
        <div class="flex justify-between">
          <button @click="show_modal = false" class="btn bg-Grayish-Blue "> NO, CANCEL</button>
          <button @click="deleteComment" class="btn bg-Soft-Red"> YES, DELETE</button>
        </div>
      </div>
    </div>
  </Teleport>

</template>


<style lang="scss" scoped>

</style>