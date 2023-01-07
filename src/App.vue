<template>
  <h1>Peek a Vue</h1>
  <section class="game-board">
    <Card v-for="card in cardList" :key="card.id" :value="card.value" :visible="card.visible" :position="card.position"
      @select-card="flipCard" />
  </section>
</template>

<script>
import { ref } from 'vue';
import Card from "./components/Card.vue";

export default {
  name: "App",
  components: {
    Card,
  },
  setup() {
    const cardList = ref([]);

    for (let i = 0; i < 16; i++) {
      cardList.value.push({
        id: i,
        value: i,
        visible: false,
        position: i
      });
    }

    const flipCard = (position) => {
      cardList.value[position].visible = !cardList.value[position].visible;
    };

    return { cardList, flipCard };
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.game-board {
  display: grid;
  justify-content: center;
  grid-template-columns: 100px 100px 100px 100px;
  grid-template-rows: 100px 100px 100px 100px;
  grid-column-gap: 30px;
  grid-row-gap: 30px;
}
</style>
