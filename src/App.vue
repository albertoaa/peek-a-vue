<template>
  <h1 class="sr-only">Peek-a-Vue</h1>
  <img src="/images/peek-a-vue-title.png" alt="peek-a-vue-title" class="title" />
  <transition-group tag="section" class="game-board" name="shuffle-card">
    <Card
      v-for="card in cardList"
      :key="`${card.value}-${card.variant}`"
      :value="card.value"
      :visible="card.visible"
      :position="card.position"
      :matched="card.matched"
      @select-card="flipCard"
    />
  </transition-group>
  <h2>{{ status }}</h2>
  <button @click="restartGame" class="button"><img src="/images/restart.svg" alt="Restart Icon" />Restart Game</button>
</template>

<script>
import { computed, ref, watch } from 'vue';
import Card from './components/Card.vue';
import _ from 'lodash';

export default {
  name: 'App',
  components: {
    Card,
  },
  setup() {
    const cardList = ref([]);
    const cardItems = ['bat', 'candy', 'cauldron', 'cupcake', 'ghost', 'pumpkin', 'moon', 'witch-hat'];

    cardItems.forEach((item) => {
      cardList.value.push({
        value: item,
        varian: 1,
        visible: false,
        position: null,
        matched: false,
      });
      cardList.value.push({
        value: item,
        variant: 2,
        visible: false,
        position: null,
        matched: false,
      });
    });

    cardList.value = cardList.value.map((card, index) => {
      return {
        ...card,
        position: index,
      };
    });

    const userSelection = ref([]);
    const status = computed(() => {
      if (remainingPairs.value === 0) {
        return 'You won!';
      } else {
        return `Remaining Pairs: ${remainingPairs.value}`;
      }
    });
    const remainingPairs = computed(() => {
      const remainingCards = cardList.value.filter((card) => !card.matched);

      return remainingCards.length / 2;
    });

    const restartGame = () => {
      cardList.value = _.shuffle(cardList.value);

      cardList.value = cardList.value.map((card, index) => {
        return {
          ...card,
          position: index,
          visible: false,
          matched: false,
        };
      });
      console.log(cardList.value);
    };

    const flipCard = (payload) => {
      cardList.value[payload.position].visible = true;

      if (userSelection.value[0]) {
        if (userSelection.value[0].position === payload.position && userSelection.value[0].faceValue === payload.faceValue)
          return;
        else userSelection.value[1] = payload;
      } else {
        userSelection.value[0] = payload;
      }
    };

    watch(
      userSelection,
      (currentValue) => {
        if (currentValue.length == 2) {
          const cardOne = currentValue[0];
          const cardTwo = currentValue[1];

          if (cardOne.faceValue === cardTwo.faceValue) {
            cardList.value[cardOne.position].matched = true;
            cardList.value[cardTwo.position].matched = true;
          } else {
            setTimeout(() => {
              cardList.value[cardOne.position].visible = false;
              cardList.value[cardTwo.position].visible = false;
            }, 1000);
          }

          userSelection.value.length = 0;
        }
      },
      { deep: true }
    );

    return { cardList, flipCard, userSelection, status, restartGame };
  },
};
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
}

h1 {
  margin-top: 0;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-image: url('../public/images/page-bg.png');
  background-color: #00070c;
  height: 100vh;
  color: #fff;
  padding-top: 60px;
}

.game-board {
  display: grid;
  justify-content: center;
  grid-template-columns: repeat(4, 130px);
  grid-template-rows: repeat(4, 130px);
  grid-column-gap: 30px;
  grid-row-gap: 30px;
}

.button {
  background-color: orange;
  color: white;
  padding: 0.75rem 0.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
  font-weight: bold;
}

.button img {
  padding-right: 5px;
}

.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

.title {
  margin-bottom: 30px;
}

.shuffle-card-move {
  transition: transform 0.8s ease-in;
}
</style>
