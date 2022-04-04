<script setup>
import { computed } from '@vue/reactivity';
import { ref,  } from 'vue';
const emit = defineEmits()
const props = defineProps(['place_holder', 'purpose', 'content', ])

const new_content = ref('')
// const updated_content = computed({
//   get: () => {
//     if (props.content) {
//       return new_content.value + props.content
//     }else{
//       return new_content.value
//     }
//   },
//   set: (update) => {
//     if (props.content) {
//       return props.content = props.content + update
//     }
//   }
// })
const submit = ()=> {
  emit('action', new_content.value)
  new_content.value = ''
}
</script>

<template>
  <div class="bg-white rounded-lg mx-4 p-4 mt-8 space-y-4 text-Grayish-Blue">
    <span v-if="purpose">{{purpose}}</span>
    <textarea v-model="new_content" :placeholder="place_holder" class="border-2 rounded-lg h-28 w-full p-4"></textarea>
    
    <div class="flex justify-between items-center">
      <slot name="avatar"></slot>
      <button @click="submit" class="btn bg-Moderate-blue">
        <slot></slot>
      </button>
    </div>
  </div>
</template>