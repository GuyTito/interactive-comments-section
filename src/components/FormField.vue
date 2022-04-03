<script setup>
import { ref } from 'vue';
const emit = defineEmits()
const props = defineProps(['place_holder', 'to', ])

const new_content = ref('')
const submit = ()=> {
  emit('send', new_content.value)
  new_content.value = ''
}
</script>

<template>
  <div class="bg-white rounded-lg mx-4 p-4 mt-8 space-y-4 text-Grayish-Blue">
    <span v-if="to">Replying to @<span>{{to}}</span></span>
    <textarea v-model="new_content" :placeholder="place_holder" class="border-2 rounded-lg h-28 w-full p-4"></textarea>
    
    <div class="flex justify-between items-center">
      <slot name="avatar"></slot>
      <button @click="submit" class="btn bg-Moderate-blue">
        <slot></slot>
      </button>
    </div>
  </div>
</template>