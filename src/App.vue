<script setup lang="ts">
import { computed, onMounted, ref, watch } from 'vue'
import ExpenseItem from './components/ExpenseItem.vue'
import ExpenseForm from './components/ExpenseForm.vue'

const uniqueId = ref(Number(localStorage.getItem('vue-uid')) ?? 0)

const expenses = ref<Expense[]>([])
const submitCount = ref(0)

const retrieveExpenses = () => {
  let data = localStorage.getItem('vue-expenses')
  return data ? JSON.parse(data) : []
}

const storeExpenses = () => {
  localStorage.setItem('vue-expenses', JSON.stringify(expenses.value))
}

const addExpense = (name: string, amount: string) => {
  let newExpense: Expense = {
    id: uniqueId.value,
    name,
    amount
  }
  expenses.value.push(newExpense)

  submitCount.value++
  uniqueId.value++
}

const removeExpense = (index: number) => {
  expenses.value = expenses.value.filter((_, idx) => idx !== index)
}

const totalAmount = computed(() => {
  return expenses.value.reduce((a, item) => a + parseFloat(item.amount), 0)
})

watch(expenses, storeExpenses, { deep: true })
watch(uniqueId, (newId) => localStorage.setItem('vue-uid', newId.toString()))

onMounted(() => {
  expenses.value = retrieveExpenses()
})
</script>

<template>
  <main class="max-w-2xl mx-auto py-8 px-4">
    <div>
      <h1 class="font-bold text-3xl">Expense Tracker</h1>

      <ExpenseForm :key="submitCount" @submit="addExpense" />
    </div>

    <div class="flex items-center justify-between mt-8">
      <h1 class="font-semibold text-lg">Expenses</h1>
      <p>
        Total:
        <span class="font-bold text-lg">RM {{ totalAmount.toFixed(2) }}</span>
      </p>
    </div>

    <template v-if="expenses.length > 0">
      <ExpenseItem
        v-for="(expense, idx) in expenses"
        :key="expense.id"
        :expense="expense"
        :index="idx"
        @remove="removeExpense"
      />
    </template>

    <div v-else>
      <p>No expenses added</p>
    </div>
  </main>
</template>
