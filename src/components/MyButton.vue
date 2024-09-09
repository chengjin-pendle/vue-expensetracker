<script setup lang="ts">
const props = defineProps({
  label: {
    type: String,
    default: 'Yes'
  },
  disabled: Boolean
})
</script>

<template>
  <button
    class="bg-green-500 px-6 py-1 rounded-lg mt-2 font-medium text-white"
    :class="{ 'opacity-50': disabled }"
    :disabled="disabled"
  >
    {{ label }}
  </button>
</template>
