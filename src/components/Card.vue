<template>
  <div class="card" @click="selectCard">
    <div v-if="visible" class="card-face is-front">
      {{ value }}
      <img v-if="matched" src="../../public/images/checkmark.svg" class="icon-checkmark" />
    </div>
    <div v-else class="card-face is-back"></div>
  </div>
</template>

<script>
export default {
  name: 'Card-Component',
  props: {
    matched: {
      type: Boolean,
      default: false,
    },
    position: {
      type: Number,
      required: true,
    },
    value: {
      type: Number,
      required: true,
    },
    visible: {
      type: Boolean,
      default: false,
    },
  },
  setup(props, context) {
    const selectCard = () => {
      context.emit('select-card', { position: props.position, faceValue: props.value });
    };

    return { selectCard };
  },
};
</script>

<style>
.card {
  position: relative;
}

.card-face {
  width: 100%;
  height: 100%;
  position: absolute;
  border-radius: 10px;
}

.card-face.is-front {
  background-image: url('../../public/images/card-bg.png');
  color: white;
}

.card-face.is-back {
  background-color: blue;
  color: white;
  background-image: url('../../public/images/card-bg-empty.png');
}

.icon-checkmark {
  position: absolute;
  right: 5px;
  bottom: 5px;
}
</style>
