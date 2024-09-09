<script lang="ts" setup>
import { ref, useTemplateRef, type InputHTMLAttributes } from 'vue'
interface MyInputProps extends /* @vue-ignore */ InputHTMLAttributes {}
const props = defineProps<MyInputProps>()
const model = defineModel()

const localRef = ref()

defineExpose({ focus: () => localRef.value.focus(), blur: () => localRef.value.blur() })
</script>

<template>
  <input class="border-2 px-4 py-1 rounded-lg" type="text" v-model="model" ref="localRef" />
</template>
