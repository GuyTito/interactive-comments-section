<script setup>

</script>


<template>
  <button class="font-bold flex items-center space-x-2">
    <i> 
      <slot name="icon"></slot> 
    </i>
    <span><slot></slot></span>
  </button>
</template>