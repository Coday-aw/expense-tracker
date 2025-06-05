<script setup>
import { defineProps } from "vue";

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
  deleteTransaction: {
    type: Function,
    required: true,
  },
});
</script>

<template>
  <div>
    <p class="font-bold">Recent transactions</p>
    <div class="h-[2px] bg-gray-300 mb-2"></div>
    <ul>
      <li
        v-for="(transaction, index) in transactions"
        :key="index"
        :class="[
          ' relative group flex justify-between border-r-4 p-1 mb-2 bg-white shadow-sm rounded-sm',
          transaction.category === 'expense'
            ? 'border-red-600'
            : 'border-green-600',
        ]"
      >
        <span class="flex justify-center items-center">
          <button
            @click="deleteTransaction(index)"
            class="absolute left-[-30px] bg-red-600 px-2 py-1 text-white cursor-pointer opacity-0 group-hover:opacity-100"
          >
            X
          </button>
          {{ transaction.text }}
        </span>

        <span
          >{{ transaction.category === "expense" ? "-" : "" }} ${{
            transaction.amount
          }}</span
        >
      </li>
    </ul>
  </div>
</template>
