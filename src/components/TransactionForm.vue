<script setup>
import { ref, defineProps } from "vue";
const newTransaction = ref("");
const amount = ref("");
const transactionCategory = ref("");

const props = defineProps({
  addTransaction: {
    type: Function,
    required: true,
  },
});
</script>

<template>
  <div>
    <h2 class="font-bold">Add new transaction</h2>
    <div class="bg-gray-300 h-[2px] mb-2"></div>
    <form @submit.prevent="addTransaction" class="flex flex-col gap-2">
      <label for="newTransaction">Text</label>
      <input
        class="bg-white px-2 py-1 rounded-sm"
        type="text"
        placeholder="Enter text"
        name="newTransaction"
        id="newTransaction"
        v-model="newTransaction"
      />
      <label for="amount">Amount</label>
      <input
        class="bg-white px-2 py-1 rounded-sm"
        type="text"
        placeholder="Enter amount"
        name="amount"
        id="amount"
        v-model="amount"
      />

      <select
        v-model="transactionCategory"
        class="bg-white text-black mb-5"
        name="transactionCategory "
        id="transactionCategory"
      >
        <option value="">Select category</option>
        <option value="income">Income</option>
        <option value="expense">Expense</option>
      </select>

      <button type="submit" class="bg-green-600 p-1 text-white">
        Add transaction
      </button>
    </form>
  </div>
</template>
