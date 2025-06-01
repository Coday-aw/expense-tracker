<script setup>
import { ref, computed } from "vue";
const newTransition = ref("");
const amount = ref("");
const transitionCategory = ref("");

const transitions = ref(
  JSON.parse(localStorage.getItem("transitions") || "[]")
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

const balance = computed(() => income.value - expense.value);
</script>

<template>
  <div class="flex justify-center items-center h-screen">
    <div class="flex flex-col gap-5 w-[300px]">
      <h2 class="font-bold text-2xl">Expense Tracker</h2>
      <div>
        <p class="font-medium">YOUR BALANCE</p>
        <P class="font-bold text-2xl">${{ balance }}</P>
      </div>
      <div class="flex justify-between gap-5 bg-white shadow-sm px-5 py-2">
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
        <p class="font-bold">History</p>
        <div class="h-[2px] bg-gray-300 mb-2"></div>
        <ul>
          <li
            v-for="(transition, index) in transitions"
            :key="index"
            :class="[
              'flex justify-between border-r-4 p-1 mb-2 bg-white shadow-sm',
              transition.category === 'expense'
                ? 'border-red-600'
                : 'border-green-600',
            ]"
          >
            <span>
              <button class="bg-red-600 px-2 text-white hidden hover:block">
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
            class="bg-white px-2 py-1"
            type="text"
            placeholder="Enter text"
            name="newTransition"
            id="newTransition"
            v-model="newTransition"
          />
          <label for="amount">Amount</label>
          <input
            class="bg-white px-2 py-1"
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
