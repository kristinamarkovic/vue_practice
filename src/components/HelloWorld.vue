<template>
  <div>
      <div class="wrap">
        <div class="wrap_flexxx" v-for="(diff, index) in difficulty" :key="index">
          <input type="radio"
                :value="choosenTiming"
                name="diffValues"
                :class="[gameStart ? 'disabled' : '']"
                @input="chooseDificulty(diff.timing)"
                class="radioBtn"/>{{ diff.name}}
        </div>
        <input type="button" class="btnStart" @click="startGame()" :value="[ gameStart ? 'Stop' : 'Start Game']">
        <!-- Random numbers-->
        <span v-if="choosenNumber">{{ choosenNumber }}</span>
      </div>
      <div class="wrap_flex" v-for="(letter, letterIndex) in lettersArray" :key="letterIndex" :class="[]">
          <div>
            <span class="upperText">{{ letter }}</span> (<span > {{ letterIndex+1 }}</span>)
          </div>
      </div>
  </div>
</template>

<script>
export default {
  name: 'Game',
  data: function () {
    return {
      difficulty: [
        {
          name: 'Easy',
          timing: 5000,
        },
        {
          name: 'Medium',
          timing: 3500,
        },
        {
          name: 'Hard',
          timing: 2000,
        }
      ],

      lettersArray: null,
      numbersArray: null,
      choosenTiming: null,
      choosenNumber: null,
      gameStart: false,
      intervalID: null,
    }
  },
  created: function () {
    this.lettersArray = [...'abcdefghijklmnopqrstuvwxyz'];
    this.numbersArray = [...Array(26).keys()];
  },
  methods: {
    chooseDificulty(timing) {
      this.choosenTiming = timing;
    },
    startGame() {
      if(!this.gameStart) {
        //filter array remove duplicates
        this.gameStart = true;
        const self = this;

        this.intervalID = setInterval(function() {
            self.choosenNumber = self.numbersArray[Math.floor(Math.random()*26)];
        }, this.choosenTiming);
      }
      else {
        clearInterval(this.intervalID);
        this.gameStart = false;
      }
    },
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="css" scoped>
.wrap_flex {
  display: flex;
  justify-content: space-between;
}
.upperText {
  text-transform: uppercase;
}
.radioBtn.disabled {
  pointer-events: none;

}
</style>
