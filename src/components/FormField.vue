<script setup>
import { ref } from 'vue';
const emit = defineEmits()
const props = defineProps(['place_holder', ])

const new_content = ref('')
const submit = ()=> {
  emit('send', new_content.value)
  new_content.value = ''
}
</script>

<template>
  <div class="bg-white rounded-lg mx-4 p-4 mt-8 space-y-4 text-Grayish-Blue">
    <textarea v-model="new_content" :placeholder="place_holder" class="border-2 rounded-lg h-28 w-full p-4"></textarea>
    
    <div class="flex justify-between items-center">
      <slot name="avatar"></slot>
      <button @click="submit" class="bg-Moderate-blue text-white py-2 px-6 rounded-lg">
        <slot></slot>
      </button>
    </div>
  </div>
</template>