<template>
  <div class="container">
    <h1>Jackpot</h1>
    <div :key="finalRolls.length" class="game_block">
      <roll-box
        v-for="(s, index) in slot"
        :key="index"
        :roll="finalRolls[index]"
        :is-rolling="isRolling"
      />
    </div>
    <button class="btn" :disabled="isRolling || credit <= 0" @click="handleRoll">Roll</button>
    <button
        class="btn"
        id="btn-cash"
        :disabled="isCashDisabled || credit <= 0"
        @mouseover="handleHoverCash"
        @click="emptyCash">CASH OUT
    </button>
    <div>Your credit is
      <h4 class="d-inline">
        {{ credit }}
      </h4>
    </div>
  </div>
</template>

<script>
import rollBox from './roll-box';

export default {
  name: 'jackpotComponent',
  data () {
    return {
      isCashDisabled: false,
      isRolling: false,
      slot: 3,
      credit: 10,
      finalRolls: [],
      selectedRolls: [],
      rolls: [
        {
          id: 1,
          name: 'cherry',
          img: '/img/cherry.svg',
          reward: 10
        },
        {
          id: 2,
          name: 'lemon',
          img: '/img/lemon.svg',
          reward: 20
        },
        {
          id: 3,
          name: 'orange',
          img: '/img/orange.svg',
          reward: 30
        },
        {
          id: 4,
          name: 'watermelon',
          img: '/img/watermelon.svg',
          reward: 40
        }
      ],
      directions: ['right', 'left', 'up', 'down']
    }
  },
  components: {
    rollBox
  },
  methods: {
    probability (percent) {
      return !!percent && Math.random() <= percent;
    },
    checkWin () {
      return this.selectedRolls.every((val, i, arr) => val === arr[0]);
    },
    roll () {
      this.selectedRolls = [];
      this.isRolling = true;
      for (let i = 0; i < this.slot; i++) {
        const random = Math.floor(Math.random() * this.rolls.length);
        this.selectedRolls.push(this.rolls[random]);
      }
    },
    calculateCredit () {
      this.checkWin() ? this.increaseCredit(): this.decreaseCredit();
    },
    increaseCredit () {
      this.credit = this.credit + this.selectedRolls[0].reward;
    },
    decreaseCredit () {
      this.credit--;
    },
    showRolls () {
      this.finalRolls = [];
      for (let i = 0; i < this.selectedRolls.length; i++) {
        setTimeout(() => {
          this.finalRolls.push(this.selectedRolls[i]);
          if (i === this.slot - 1) {
            this.calculateCredit();
            this.isRolling = false;
          }
        }, 1000 * (i + 1))
      }
    },
    cheat (percent) {
      if (this.probability(percent)) {
        this.roll();
        let isWinner = this.checkWin();
        if (isWinner) {
          this.roll();
        }
      }
    },
    handleRoll () {
      this.roll();
      let isWin = this.checkWin();
      if (40 <= this.credit && this.credit <= 60 && isWin) {
        console.log('30%')
        this.cheat(0.8);
      } else if (this.credit > 60 && isWin) {
        this.cheat(0.6);
        isWin = this.checkWin();
        if (isWin) {
          this.roll();
        }
      }
      this.showRolls();
    },
    emptyCash () {
      this.credit = 0;
    },
    moveCashByChance () {
      const random = Math.floor(Math.random() * this.directions.length);
      const foundedDirection = this.directions[random];
      const btn = document.getElementById('btn-cash');
      switch (foundedDirection) {
        case 'up':
          btn.style.transform = 'translateY(-300px)';
          break;
        case 'down':
          btn.style.transform = 'translateY(300px)';
          break;
        case 'right':
          btn.style.transform = 'translateX(-300px)';
          break;
        case 'left':
          btn.style.transform = 'translateX(300px)';
          break;
      }
    },
    handleHoverCash () {
      if (this.probability(0.5)) {
        this.moveCashByChance();
      }
      if (this.probability(0.4)) {
        this.isCashDisabled = true;
      }
    }
  }
}
</script>

<style scoped>

h2 {
  color: #60208b;
}

.container {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.game_block {
  width: 600px;
  height: 300px;
  box-sizing: border-box;
  /*border: 1px solid #60208b;*/
  display: flex;
  margin-top: 20px;
  margin-bottom: 20px;
  overflow: hidden;
}

.btn {
  font-size: 24px;
  padding: 8px;
  background-color: inherit;
  color: #60208b;
  border: 1px solid #60208b;
  outline: none;
  cursor: pointer;
  margin-bottom: 30px;
  transition: 0.3s ease;
}

.btn:disabled {
  pointer-events: none;
  opacity: 0.7;
}

.btn:hover {
  transform: translateY(2px);
  background-color: #60208b;
  color: #fff;
}

.d-inline {
  display: inline;
}

</style>
