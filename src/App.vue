<script>
import Header from "@/components/Header.vue";
import Balance from "@/components/Balance.vue";
import IncomeExpense from "@/components/IncomeExpense.vue";
import TransactionList from "@/components/TransactionList.vue";
import AddTransaction from "@/components/AddTransaction.vue";

import { useToast } from "vue-toastification";

export default {
  components: {
    Header,
    Balance,
    IncomeExpense,
    TransactionList,
    AddTransaction,
  },

  data() {
    return {
      transactions: [],
      toast: useToast(),
      transactionsSave: [],
    };
  },

  mounted() {
    this.getTransactionsToLocalStorage();
  },

  computed: {
    total() {
      return this.transactions.reduce((acc, transaction) => {
        return acc + transaction.amount;
      }, 0);
    },
    income() {
      return this.transactions
        .filter((transaction) => transaction.amount > 0)
        .reduce((acc, transaction) => {
          return acc + transaction.amount;
        }, 0)
        .toFixed(2);
    },
    expense() {
      return this.transactions
        .filter((transaction) => transaction.amount < 0)
        .reduce((acc, transaction) => {
          return acc + transaction.amount;
        }, 0);
    },
  },

  methods: {
    HandleTransactionSubmitted(transactionData) {
      this.transactions.push({
        id: this.generateUniqueId(),
        text: transactionData.text,
        amount: transactionData.amount,
      });

      this.saveTransactionsToLocalStorage();

      this.toast.success("Transaction Added");
    },

    generateUniqueId() {
      return Math.floor(Math.random() * 1000000);
    },

    HandleTransactionDelete(id) {
      this.transactions = this.transactions.filter(
        (transaction) => transaction.id !== id
      );

      this.saveTransactionsToLocalStorage();

      this.toast.success("Transaction deleted");
    },

    getTransactionsToLocalStorage() {
      this.transactionsSave = JSON.parse(localStorage.getItem("transactions"));

      if (this.transactionsSave) {
        this.transactions = this.transactionsSave;
      }
    },

    saveTransactionsToLocalStorage() {
      localStorage.setItem("transactions", JSON.stringify(this.transactions));
    },
  },
};
</script>
<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpense :income="+income" :expense="+expense" />
    <TransactionList
      :transactions="transactions"
      @deleteTransaction="HandleTransactionDelete"
    />
    <AddTransaction @transactionSubmitted="HandleTransactionSubmitted" />
  </div>
</template>
