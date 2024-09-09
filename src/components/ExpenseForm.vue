<script lang="ts" setup>
import { ref } from 'vue'
import MyTextInput from './MyTextInput.vue'
import MyButton from './MyButton.vue'

const expenseName = ref('')
const expenseAmount = ref('')

const emit = defineEmits<{ (event: 'submit', name: string, amount: string): void }>()

const handleSubmit = () => {
  emit('submit', expenseName.value, expenseAmount.value)
}
</script>

<template>
  <form @submit.prevent="handleSubmit" class="mt-4">
    <div class="flex sm:flex-row flex-col gap-2 w-full md:w-3/4">
      <MyTextInput class="flex-1" v-model="expenseName" required placeholder="Expense" />

      <MyTextInput
        class="flex-1"
        ref="amountInputRef"
        v-model="expenseAmount"
        required
        type="number"
        step=".01"
        placeholder="Amount"
      />
    </div>

    <MyButton class="text-sm" type="submit">Add</MyButton>
  </form>
</template>
