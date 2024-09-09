<script setup lang="ts">
import { computed, onMounted, ref, useTemplateRef, watch } from 'vue'
import MyButton from './components/MyButton.vue'
import ExpenseItem from './components/ExpenseItem.vue'
import MyTextInput from './components/MyTextInput.vue'

const expenseName = ref('')
const expenseAmount = ref('')
const expenses = ref<Expense[]>([])
const amountInputRef = ref<HTMLInputElement>()
const formRef = ref<HTMLFormElement>()

const retrieveExpenses = () => {
  let data = localStorage.getItem('vue-expenses')
  return data ? JSON.parse(data) : []
}

const storeExpenses = () => {
  localStorage.setItem('vue-expenses', JSON.stringify(expenses.value))
}

const addExpense = () => {
  let newExpense: Expense = {
    name: expenseName.value,
    amount: expenseAmount.value
  }
  expenses.value.push(newExpense)

  expenseName.value = ''
  expenseAmount.value = ''
  amountInputRef.value?.blur()
}

const removeExpense = (index: number) => {
  expenses.value = expenses.value.filter((_, idx) => idx !== index)
}

const focusAmountInput = () => {
  if (amountInputRef.value) amountInputRef.value.focus()
}

const buttonDisabled = computed(() => {
  return !(Boolean(expenseName.value) && Boolean(expenseAmount.value))
})

const totalAmount = computed(() => {
  return expenses.value.reduce((a, item) => a + parseFloat(item.amount), 0)
})

watch(expenses, storeExpenses, { deep: true })

onMounted(() => {
  expenses.value = retrieveExpenses()
})
</script>

<template>
  <main class="max-w-2xl mx-auto py-8 px-4">
    <div>
      <h1 class="font-bold text-3xl">Expense Tracker</h1>

      <form ref="formRef" @submit.prevent="addExpense" class="mt-4">
        <div class="flex sm:flex-row flex-col gap-2 w-full md:w-3/4">
          <MyTextInput
            class="flex-1"
            v-model="expenseName"
            required
            placeholder="Expense"
            @keypress.enter="focusAmountInput"
          />
          <MyTextInput
            class="flex-1"
            ref="amountInputRef"
            v-model="expenseAmount"
            required
            step=".01"
            placeholder="Amount"
            @keypress.enter="() => formRef?.requestSubmit()"
          />
        </div>

        <MyButton label="Add" :disabled="buttonDisabled" class="text-sm" />
      </form>
    </div>

    <div class="flex items-center justify-between mt-8">
      <h1 class="font-semibold text-lg">Expenses</h1>
      <p>
        Total:
        <span class="font-bold text-lg">RM {{ totalAmount.toFixed(2) }}</span>
      </p>
    </div>

    <ExpenseItem
      v-if="expenses.length > 0"
      v-for="(expense, idx) in expenses"
      :key="idx"
      :expense="expense"
      :index="idx"
      @remove="removeExpense"
    />

    <div v-else>
      <p>No expenses added</p>
    </div>
  </main>
</template>
