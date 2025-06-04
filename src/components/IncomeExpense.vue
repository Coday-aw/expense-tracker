<script setup>
import { defineProps } from "vue";

const props = defineProps({
  income: Number,
  expense: Number,
});
</script>

<template>
  <div
    class="flex justify-between gap-5 bg-white shadow-sm px-5 py-2 rounded-sm"
  >
    <div>
      <p>Income</p>
      <p class="text-green-500">${{ income }}</p>
    </div>
    <div class="w-[2px] bg-gray-100"></div>
    <div>
      <p>Expense</p>
      <p class="text-red-600">-${{ expense }}</p>
    </div>
  </div>
</template>
