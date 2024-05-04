<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpense :income="+income" :expenses="-expenses" />
    <TransactionHistory
      :transactions="transactions"
      @transactionDeleted="handleDelete"
    />
    <AddTransaction @transactionSubmitted="handleSubmit" />
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpense from "./components/IncomeExpense.vue";
import TransactionHistory from "./components/TransactionHistory.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { ref, computed } from "vue";
import { useToast } from "vue-toast-notification";
import "vue-toast-notification/dist/theme-sugar.css";

const $toast = useToast();

const transactions = ref([
  {
    id: 1,
    text: "Bookshelf",
    amount: -129.99,
  },
  {
    id: 2,
    text: "Smartphone Case",
    amount: -24,
  },
  {
    id: 3,
    text: "Paycheck",
    amount: 1200,
  },
  {
    id: 4,
    text: "Headphones",
    amount: -59,
  },
  {
    id: 5,
    text: "Blanket",
    amount: -39,
  },
]);

// Get total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

// Get income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2);
});

// Get expenses
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2);
});

// Handle Transaction Submit
const handleSubmit = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });
  $toast.success("Transaction Added!");
};

// Generate unique ID
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
};

//Delete Transaction
const handleDelete = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );
  $toast.success("Transaction Deleted!");
};
</script>
