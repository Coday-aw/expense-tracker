<script setup>
import { defineProps } from "vue";

const props = defineProps({
  transitions: {
    type: Array,
    required: true,
  },
  deleteTransition: {
    type: Function,
    required: true,
  },
});
</script>

<template>
  <div>
    <p class="font-bold">Recent transitions</p>
    <div class="h-[2px] bg-gray-300 mb-2"></div>
    <ul>
      <li
        v-for="(transition, index) in transitions"
        :key="index"
        :class="[
          ' relative group flex justify-between border-r-4 p-1 mb-2 bg-white shadow-sm rounded-sm',
          transition.category === 'expense'
            ? 'border-red-600'
            : 'border-green-600',
        ]"
      >
        <span class="flex justify-center items-center">
          <button
            @click="deleteTransition(index)"
            class="absolute left-[-30px] bg-red-600 px-2 py-1 text-white cursor-pointer opacity-0 group-hover:opacity-100"
          >
            X
          </button>
          {{ transition.text }}
        </span>

        <span
          >{{ transition.category === "expense" ? "-" : "" }} ${{
            transition.amount
          }}</span
        >
      </li>
    </ul>
  </div>
</template>
