<template>
  <div class="card">Card</div>
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
.card {
  border: 5px solid #ccc;
}
</style>
