<template>
<div class="container">
  <transition name="fade-in">
    <div v-if="won" class="wonModal">
      <div class="innerModal">
        <h2>Thanks for playing!</h2><br><br>
        <h2 v-if="hiddenStates.length >= 1">
          You missed the following states:
          {{this.hiddenStates.toString().replace(/,/g, ", ")}}
        </h2>
        <br><br><button v-on:click="initialState()">Play Again!</button><br>
      </div>
    </div>
  </transition>

  <div>
    <div
      style="padding:10px; display: flex; flex-direction:column; justify-content: center;
      align-items: center; background-color: white; box-shadow: 0 4px 6px 0 hsla(0, 0%, 0%, 0.4);"
    >
      <input
        style="width: 80%;"
        v-on:keyup="check()"
        v-on:keydown="check()"
        :disabled="disabled"
        v-model="guessText"
        placeholder="State..."
      ><br>
      <div style="margin-bottom:5px;">
        <h2 style="display: inline;">{{guessedStates.length}} / </h2>
        <input
          v-model="target"
          style="width:50px; display: inline; background: none; padding: 5px;
          font-family: 'Gloria Hallelujah', cursive;font-size: 1.5em;"
        />
        <h4 style="display: inline;">← set your target!</h4>
        <button @click="iQuit()">Give Up</button>
      </div>
    </div>
    <div style="display: flex; flex-wrap: wrap; padding: 10px;">
      <h2
        v-for="state, i in sortedStates.sort()"
        :key=i
        style="background-color: white; margin:5px; padding:5px;"
      >
        {{state}}
      </h2>
    </div>
  </div>

  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      hiddenStates: ['Alabama', 'Alaska', 'Arizona', 'Arkansas', 'California', 'Colorado', 'Connecticut', 'Delaware', 'Florida', 'Georgia', 'Hawaii', 'Idaho', 'Illinois', 'Indiana', 'Iowa', 'Kansas', 'Kentucky', 'Louisiana', 'Maine', 'Maryland', 'Massachusetts', 'Michigan', 'Minnesota', 'Mississippi', 'Missouri', 'Montana', 'Nebraska', 'Nevada', 'New Hampshire', 'New Jersey', 'New Mexico', 'New York', 'North Carolina', 'North Dakota', 'Ohio', 'Oklahoma', 'Oregon', 'Pennsylvania', 'Rhode Island', 'South Carolina', 'South Dakota', 'Tennessee', 'Texas', 'Utah', 'Vermont', 'Virginia', 'Washington', 'West Virginia', 'Wisconsin', 'Wyoming'],
      guessedStates: [],
      guessText: '',
      won: false,
      target: 50,
      disabled: false,
    };
  },
  methods: {
    check() {
      if (this.hiddenStates.includes(this.capitalGuess)) {
        this.hiddenStates = this.hiddenStates.filter((item) => item !== this.capitalGuess);
        this.guessedStates.push(this.capitalGuess);
        this.guessText = '';
        this.checkFinished();
      }
    },
    checkFinished() {
      if (this.guessedStates.length >= this.target) {
        this.won = !this.won;
        this.disabled = !this.disabled;
      }
    },
    iQuit() {
      this.won = !this.won;
    },
    initialState() {
      // eslint-disable-next-line no-unused-expressions
      this.hiddenStates = ['Alabama', 'Alaska', 'Arizona', 'Arkansas', 'California', 'Colorado', 'Connecticut', 'Delaware', 'Florida', 'Georgia', 'Hawaii', 'Idaho', 'Illinois', 'Indiana', 'Iowa', 'Kansas', 'Kentucky', 'Louisiana', 'Maine', 'Maryland', 'Massachusetts', 'Michigan', 'Minnesota', 'Mississippi', 'Missouri', 'Montana', 'Nebraska', 'Nevada', 'New Hampshire', 'New Jersey', 'New Mexico', 'New York', 'North Carolina', 'North Dakota', 'Ohio', 'Oklahoma', 'Oregon', 'Pennsylvania', 'Rhode Island', 'South Carolina', 'South Dakota', 'Tennessee', 'Texas', 'Utah', 'Vermont', 'Virginia', 'Washington', 'West Virginia', 'Wisconsin', 'Wyoming'];
      this.guessedStates = [];
      this.guessText = '';
      this.won = false;
      this.target = 50;
      this.disabled = false;
    },
  },
  computed: {
    sortedStates() {
      return this.guessedStates;
    },
    capitalGuess() {
      return this.guessText.replace(
        /\w\S*/g,
        (txt) => txt.charAt(0).toUpperCase() + txt.substr(1).toLowerCase(),
      );
    },
  },
};
</script>

<style>
* {box-sizing: border-box; margin: 0; padding: 0;}

body {
  background-image: url('./assets/starring.png'),
    repeating-linear-gradient(to left,
    rgba(205,0,0,0.4), rgba(205,0,0,0.4) 50%, rgba(0,0,205,0.4) 30px, rgba(0,0,205,0.4) 100%);
  background-color: rgba(0,0,0);
}

.container {
  display: flex; flex-direction: column;
}

h1, h2, h3, h4, h5, h6, p { font-family: 'Gloria Hallelujah', cursive; }

input {
  padding: 10px 20px; outline: none; border: 3px solid #666;
}

hr {
  border: 0;
  height: 1px;
  background-image: linear-gradient(to right,rgba(0, 0, 0, 0),rgba(0, 0, 0, 0.75),rgba(0, 0, 0, 0));
}

.fade-in-enter-active { animation: fade-in 0.5s reverse; }
.fade-in-leave-active { animation: fade-in 0.5s; }

@keyframes fade-in {
    from { opacity: 1; }
    to   { opacity: 0; }
}

.wonModal {
  background-color:rgba(155, 155, 155, 0.9); width: 100%; height: 100%;
  position: fixed; bottom: 0; display: flex; justify-content: center;
  align-items: center;
}

.innerModal {
  background-color: white; height: 75%; width: 50%; padding: 10px;
  border-radius: 10px; overflow-y: scroll; overflow-x: hidden;
  display: flex; flex-direction: column; justify-content: space-evenly; align-items: center;
}

button {
  padding: 15px 30px;
  border-radius: 5px;
  border: 2px solid black;
  margin: 10px 15px;
  cursor: pointer;
}
</style>
