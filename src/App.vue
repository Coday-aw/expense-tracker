<script setup>
import { ref, computed } from "vue";
import TransactionsList from "./components/TransactionsList.vue";
import TransactionForm from "./components/TransactionForm.vue";
import IncomeExpense from "./components/IncomeExpense.vue";

const transactions = ref(
  JSON.parse(localStorage.getItem("transactions") || "[]")
);

const income = computed(() =>
  transactions.value
    .filter((t) => t.category === "income")
    .reduce((sum, t) => sum + Number(t.amount), 0)
);

const expense = computed(() =>
  transactions.value
    .filter((t) => t.category === "expense")
    .reduce((sum, t) => sum + Number(t.amount), 0)
);

const balance = computed(() => income.value - expense.value);

const addTransaction = () => {
  if (
    newTransaction.value.trim() !== "" &&
    amount.value.trim() !== "" &&
    transactionCategory !== ""
  ) {
    const newItem = {
      text: newTransaction.value,
      amount: amount.value,
      category: transactionCategory.value,
    };
    transactions.value = [...transactions.value, newItem];
    localStorage.setItem("transactions", JSON.stringify(transactions.value));
    newTransaction.value = "";
    amount.value = "";
    transactionCategory.value = "";
  }
};

const deleteTransaction = (index) => {
  const updatedtransactions = transactions.value.filter((_, i) => i !== index);
  transactions.value = updatedtransactions;
  localStorage.setItem("transactions", JSON.stringify(updatedtransactions));
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
      <IncomeExpense :income="income" :expense="expense" />
      <transactionsList
        :transactions="transactions"
        :deleteTransaction="deleteTransaction"
      />
      <TransactionForm :addTransaction="addTransaction" />
    </div>
  </div>
</template>
