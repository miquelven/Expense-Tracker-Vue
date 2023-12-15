<script>
export default {
  props: ["transactions"],
};
</script>

<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }} <span>{{ transaction.amount }}</span
      ><button
        class="delete-btn"
        @click="() => this.$emit('deleteTransaction', transaction.id)"
      >
        x
      </button>
    </li>
  </ul>
</template>
