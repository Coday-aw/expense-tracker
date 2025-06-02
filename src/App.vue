<script setup>
import { ref, computed } from "vue";
const newTransition = ref("");
const amount = ref("");
const transitionCategory = ref("");

const transitions = ref(
  JSON.parse(localStorage.getItem("transitions") || "[]")
);

const income = computed(() =>
  transitions.value
    .filter((t) => t.category === "income")
    .reduce((sum, t) => sum + Number(t.amount), 0)
);

const expense = computed(() =>
  transitions.value
    .filter((t) => t.category === "expense")
    .reduce((sum, t) => sum + Number(t.amount), 0)
);

const addTransition = () => {
  if (
    newTransition.value.trim() !== "" &&
    amount.value.trim() !== "" &&
    transitionCategory !== ""
  ) {
    const newItem = {
      text: newTransition.value,
      amount: amount.value,
      category: transitionCategory.value,
    };
    transitions.value = [...transitions.value, newItem];
    localStorage.setItem("transitions", JSON.stringify(transitions.value));
    newTransition.value = "";
    amount.value = "";
    transitionCategory.value = "";
  }
};

const balance = computed(() => income.value - expense.value);

const deleteTransition = (index) => {
  const updatedTransitions = transitions.value.filter((_, i) => i !== index);
  transitions.value = updatedTransitions;
  localStorage.setItem("transitions", JSON.stringify(updatedTransitions));
};
</script>

<template>
  <div class="flex justify-center items-center h-screen">
    <div
      class="flex flex-col gap-5 w-[400px] p-10 bg-blue-50 shadow-lg rounded-lg"
    >
      <h2 class="font-bold text-2xl text-center">Expense Tracker</h2>
      <div
        class="flex flex-col justify-center items-center bg-white p-2 shadow-sm rounded-sm"
      >
        <p class="font-medium">YOUR BALANCE</p>
        <P class="font-bold text-2xl">${{ balance }}</P>
      </div>
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
      <div>
        <h2 class="font-bold">Add new transition</h2>
        <div class="bg-gray-300 h-[2px] mb-2"></div>
        <form @submit.prevent="addTransition" class="flex flex-col gap-2">
          <label for="newTransition">Text</label>
          <input
            class="bg-white px-2 py-1 rounded-sm"
            type="text"
            placeholder="Enter text"
            name="newTransition"
            id="newTransition"
            v-model="newTransition"
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
            v-model="transitionCategory"
            class="bg-white text-black mb-5"
            name="transitionCategory "
            id="transitionCategory"
          >
            <option value="">Select category</option>
            <option value="income">Income</option>
            <option value="expense">Expense</option>
          </select>

          <button type="submit" class="bg-green-600 p-1 text-white">
            Add transition
          </button>
        </form>
      </div>
    </div>
  </div>
</template>
