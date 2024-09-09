<script lang="ts" setup>
const emit = defineEmits<{
  remove: [index: number]
}>()

const props = defineProps({
  expense: Object,
  index: Number
})

const handleClick = () => {
  if (props.index !== undefined) emit('remove', props.index)
}
</script>

<template>
  <div style="display: flex; justify-content: space-between; align-items: center" class="pt-4">
    <div>
      <p class="bg-yellow-500 rounded-full text-center px-4 font-medium">
        {{ expense?.name }}
      </p>
      <p class="mt-1">RM {{ expense?.amount }}</p>
    </div>

    <div
      @click="handleClick"
      class="bg-red-500 text-white px-2 rounded-md cursor-pointer select-none text-sm"
    >
      <p>Remove</p>
    </div>
  </div>
</template>
