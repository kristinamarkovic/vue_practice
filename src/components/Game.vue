<template>
  <div>
      <div class="wrap">
        <div class="radio_flex" v-for="(diff, index) in difficulty" :key="index">
          <span><input type="radio"
                :value="choosenTiming"
                name="diffValues"
                :class="[gameStart ? 'disabled' : '']"
                @input="chooseDificulty(diff.timing)"
                class="radioBtn"/>{{ diff.name}}
          </span>
        </div>

        <input type="button" :disabled="choosenTiming === null" class="btnStart" @click="startGame()" :value="[ gameStart ? 'Stop' : 'Start Game']">
        <!-- Random numbers-->
        <span class="randomNumber" v-if="choosenNumber">{{ choosenNumber }}</span><br>
        <input type="text" :class="[gameStart ? '' : 'disabled']" placeholder="Input letter" v-model="typedLetter" @input="checkScore($event)"/>
      </div>
      <div class="wrap_flex">
          <div class="letter" v-for="(letter, letterIndex) in lettersArray" :key="letterIndex">
            <span :class="[greenLetter[letterIndex] === true ? 'green' : '', redLetter[letterIndex] === true ? 'red' : '']" class="upperText">{{ letter }} ( {{letterIndex+1 }})</span>
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
      typedLetter: null,
      redLetter: [],
      greenLetter: [],
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
    checkScore(event) {
      this.typedLetter = event.target.value;
      const foundLetter = this.lettersArray.find(element => element === this.typedLetter.toLowerCase());
      const indexLett = this.lettersArray.findIndex(element => element === foundLetter);
      if(foundLetter)  {
        if(indexLett === this.choosenNumber) {
          console.log('Score!!!');
          this.$set(this.greenLetter, indexLett, !this.greenLetter[indexLett]);
          setTimeout(function() {
            this.typedLetter = "";
          }.bind(this), 500);
        }
        else {
          console.log('Fail!!!');
          this.$set(this.redLetter, indexLett, !this.redLetter[indexLett]);
          setTimeout(function() {
            this.typedLetter = "";
          }.bind(this), 500);
        }
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="css" scoped>
.wrap_flex {
  display: flex;
    justify-content: unset;
    width: 50%;
    margin: 0 auto;
    flex-wrap: wrap;
}
.letter {
  min-width: 65px;
  padding: 20px;

}
.radio_flex {
  display: contents;
  justify-content: space-between;
}
.btnStart {
  width: auto;
  padding: 5px 10px;
  height: 30px;
  border: 1px solid violet !important;
  color: violet;
  font-size: 12px;
  background: white;
  display: block;
  margin: 0 auto;
  position: relative;
  top: 20px;
}
.btnStart:disabled:hover{
  cursor: default;
}
input[type=text] {
  padding: 5px 10px;
  height: 15px;
  color: gray;
  border: 1px solid #dedede;
  margin-top: 25px;
}
input[type=text]:focus, input[type=text]:hover, .btnStart:focus {
  box-shadow: none;
  border: 1px solid lightskyblue;
  outline: none;
}
.btnStart:hover {
  cursor: pointer;
}
.upperText {
  text-transform: uppercase;
  font-weight: bold;
}
.radioBtn.disabled,  input[type=text].disabled {
  pointer-events: none;

}
.green {
  color: green;
}
.red {
  color: red;
}
.randomNumber {
  padding: 15px;
  font-size: 35px;
}
</style>
