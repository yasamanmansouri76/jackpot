<template>
  <div class="row">
    <div class="box">
      <img v-if="roll" :src="roll.img"/>
      <loader v-else-if="isRolling && !roll"/>
      <span v-else>Roll</span>
    </div>
  </div>
</template>

<script>
import loader from './loader';

export default {
  name: "roll-box",
  components: {
    loader
  },
  props: {
    roll: {
      required: true
    },
    isRolling: {
      type: Boolean,
      default: false,
      required: true
    }
  }
}
</script>

<style scoped>

.game_block .row {
  width: 100%;
  height: 300px;
  pointer-events: none;
  position: relative;
}

.game_block .box {
  width: 100%;
  height: 300px;
  background-size: 100%;
  background-position: center;
  background-repeat: no-repeat;
  border: 1px solid #60208b;
  box-sizing: border-box;
  display: flex;
  justify-content: center;
  align-items: center;
}

.game_block .box img {
  width: 70%;
}
</style>