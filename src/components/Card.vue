<template>
  <div class="card">Card {{ id }} {{ value }}</div>
</template>

<script>
export default {
  name: "Card-Component",
  props: {
    id: Number,
    value: String,
  },
};
</script>


<style scoped>
</style>
