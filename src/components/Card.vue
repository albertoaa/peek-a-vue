<template>
  <div class="card">{{ value }}</div>
</template>

<script>
export default {
  name: "Card-Component",
  props: {
    value: {
      type: Number,
      required: true,
    },
  },
};
</script>


<style>
.card {
  border: 5px solid #ccc;
}
</style>
