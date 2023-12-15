<template>
  <h3>Add new Transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" v-model="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount">
        Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        type="number"
        id="amount"
        v-model="amount"
        placeholder="Enter amount..."
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script>
import { useToast } from "vue-toastification";

export default {
  data() {
    return {
      text: "",
      amount: "",
      toast: useToast(),
      transactionData: [],
    };
  },

  methods: {
    onSubmit() {
      if (!this.text.trim() || !this.amount.trim()) {
        this.toast.error("Both fields must be filled");
        return;
      }

      this.transactionData = {
        text: this.text,
        amount: this.amount,
      };

      this.text = "";
      this.amount = "";

      this.$emit("transactionSubmitted", this.transactionData);
    },
  },
};
</script>
