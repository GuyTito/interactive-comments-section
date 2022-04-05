<script setup>
import { computed } from '@vue/reactivity';

const props = defineProps(['avatar_path', ])

// const img_src = computed(()=>{
//   return `/src/${props.avatar_path}`
// })

</script>


<template>
  <img :src="avatar_path" alt="avatar" class="h-8 w-8 inline">
</template>