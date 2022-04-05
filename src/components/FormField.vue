<script setup>
import { onMounted, ref,  } from 'vue';
const emit = defineEmits()
const props = defineProps(['place_holder', 'purpose', 'content', ])

const new_content = ref('')
if (props.content) new_content.value = props.content

const submit = ()=> {
  emit('action', new_content.value)
  new_content.value = ''
}

const close = () => {
  emit('close')
}

const textbox = ref(null)
// autofocus textarea when reply or edit
onMounted(()=> {
  if (props.purpose) textbox.value.focus()
})

</script>

<template>
  <div class="bg-white rounded-lg mx-4 p-4 mt-8 space-y-4 text-Grayish-Blue">
    <div v-if="purpose" class="flex justify-between items-center">
      <span>{{purpose}}</span>
      <button v-if="content" @click="close">X</button>
    </div>
    <textarea v-model="new_content" ref="textbox" :placeholder="place_holder" class="border rounded-lg h-28 w-full p-4 focus:outline-Moderate-blue" ></textarea>
    
    <div class="flex justify-between items-center">
      <slot name="avatar"></slot>
      <button @click="submit" class="btn bg-Moderate-blue">
        <slot></slot>
      </button>
    </div>
  </div>
</template>