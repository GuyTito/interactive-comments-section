<script setup>
import { computed, ref } from "@vue/reactivity";
import ReplyIcon from "./icons/ReplyIcon.vue";
import current_user from '@/composables/current_user.js'
import RDEButton from "./RDEButton.vue";
import DeleteIcon from "./icons/DeleteIcon.vue";
import EditIcon from "./icons/EditIcon.vue";
import Avatar from "./Avatar.vue";
import FormField from "./FormField.vue";
import moment from 'moment'

const props = defineProps(['comment', 'parent', ])

const ownership = computed(()=>{
  return props.comment.user.username == current_user.username
})

const show_form = ref(false)
const toggleForm = () => {
  show_form.value = !show_form.value
}

const purpose = computed(() => {
  if (props.comment.hasOwnProperty('replyingTo')) {
    return `Updating reply to @${props.comment.replyingTo}`
  }else{
    return 'Updating comment...'
  }
})

const data = ref([])
data.value = JSON.parse(localStorage.getItem('comments'))

const addReply = (new_content)=> {
  if (new_content) {
    const new_reply = {
      id: Math.floor(Date.now() * Math.random()),
      content: new_content,
      user: current_user,
      createdAt: Date.now(),
      score: 0,
      replyingTo: props.comment.user.username,
    }

    // reply to the right reply
    if (props.comment.hasOwnProperty('replyingTo')) {
      data.value.forEach(comment => {
        if (comment.id == props.parent.id) {
          props.parent.replies.push(new_reply)
          comment.replies.push(new_reply)
        }
      })
    }else{
      data.value.forEach(comment => {
        // reply to the right comment
        if (comment.id == props.comment.id) {
          props.comment.replies.push(new_reply)
          comment.replies.push(new_reply)
        }
      })
    }
    localStorage.setItem('comments', JSON.stringify(data.value))

    show_form.value = false
  }
}

const show_modal = ref(false)
// defined emit here to avoid "Extraneous..." warning
const emit = defineEmits(['delete', ])
const deleteComment = () => {
  // delete the right reply
  if (props.comment.hasOwnProperty('replyingTo')) {
    data.value.forEach(comment => {
      if (comment.id == props.parent.id) {
        props.parent.replies = props.parent.replies.filter(reply => reply.id != props.comment.id)
        comment.replies = props.parent.replies
      }
    })
    localStorage.setItem('comments', JSON.stringify(data.value))
  }else{
    data.value.forEach(comment => {
      // delete the right comment
      if (comment.id == props.comment.id) {
        // had to emit this function because a component cannot delete itself
        emit('delete', props.comment.id)
      }
    })
  }
}

const show_edit = ref(false)
const toggleEdit = () => {
  show_edit.value = !show_edit.value
}

const updateComment = (content) => {
  if (props.comment.hasOwnProperty('replyingTo')) {
    data.value.forEach(comment => {
      if (comment.id == props.parent.id) {
        comment.replies.forEach(reply =>{
          if (reply.id == props.comment.id) {
            reply.content = props.comment.content = content
          }
        })
      }
    })
    localStorage.setItem('comments', JSON.stringify(data.value))
  }else{
    data.value.forEach(comment => {
      if (comment.id == props.comment.id) {
        comment.content = props.comment.content = content
      }
    })
    localStorage.setItem('comments', JSON.stringify(data.value))
  }

  show_edit.value = false
}

// to update time without refresh
const created_at = ref(moment(props.comment.createdAt).fromNow())
const updateTime = () => created_at.value = moment(props.comment.createdAt).fromNow()
setInterval(updateTime, 1000)

</script>


<template>
  <div v-if="!show_edit" class="bg-white rounded-lg mx-4 p-4 mt-4 space-y-4 text-Grayish-Blue">
    <div class="space-x-2">
      <Avatar :avatar_path="comment.user.image.png" />
      <span class="font-bold text-Dark-blue">{{comment.user.username}}</span>
      <span v-if="ownership" class="bg-Moderate-blue py-[2px] px-1 rounded-sm text-white text-xs">you</span>
      <span class="pl-2"> {{ created_at }} </span>
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
  
        <RDEButton v-if="ownership" @click="toggleEdit">
          <template #icon>
            <EditIcon />
          </template>
          Edit
        </RDEButton>
      </div>
    </div>
  </div>

  <!-- Reply form -->
  <FormField v-if="show_form" @action="addReply" :place_holder="'Reply...'" :purpose="`Replying to @${comment.user.username}`">
    <template #avatar>
      <Avatar :avatar_path="current_user.image.png" />
    </template>
    Reply
  </FormField>

  <!-- Edit form -->
  <FormField v-if="show_edit" @action="updateComment" :purpose="purpose" :content="comment.content" @close="toggleEdit">
    <template #avatar>
      <Avatar :avatar_path="current_user.image.png" />
    </template>
    Update
  </FormField>

  <!-- Delete modal -->
  <Teleport to="body" v-if="show_modal">
    <div @click="show_modal = false" class="fixed inset-0 bg-black/40 grid place-content-center">
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