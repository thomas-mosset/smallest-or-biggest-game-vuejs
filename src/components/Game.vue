<template>
  <div class="game">

    <h1 class="title">Smallest or greatest game</h1>

    <form @submit.prevent="submitNumber()" v-if="gameStatus === false">
      <label for="number" class="label">Enter a number between 1 and 100 :</label>
      <input type="number" min="1" max="100" name="number" id="number" class="input" v-model="userNumber">
      <button type="submit" class="btn">Submit my number</button>
    </form>

    <p class="msg">{{ message }}</p>
    <p class="nbOfTriesLeft" v-if="gameStatus === false">Number of tries left : <span>{{ numberOfTry }}</span></p>

    <!-- only show if we're out of tries or that the player has won or loose -->
    <button v-if="gameStatus === true" @click="playAgain()" class="btn">Play again !</button>

  </div>
</template>

<script>
export default {
  name: 'TheGame',
  data() {
    return {
      numberToGuess: null,
      userNumber: null,
      numberOfTry: 10,
      gameStatus: false,
      message: "",
    };
  },
  created () {
    this.initializedNumberToGuess();
  },
  methods: {
    initializedNumberToGuess () {
      // initialize the min and max possible value for our random number to guess
      let minimumNumber = 1;
      let maximumNumber = 100;

      // getting a random number between 1 and 100
      this.numberToGuess = Math.floor(Math.random() * (maximumNumber - minimumNumber + 1)) + minimumNumber;

      console.log(this.numberToGuess);
    },

    submitNumber() {
      // Reset the value of our message
      this.message = "";

      if (isNaN(this.userNumber) || this.userNumber === null) {
        this.message = "You must enter a value of type number.";

      } else {
        // case : nb out of range
        if (this.userNumber < 1 || this.userNumber > 100) {
          this.message = "The submitted number must be between 1 and 100 !";

        // case : win
        } else if (this.userNumber === this.numberToGuess) {
          this.message = "Congratulations, you won !";

          this.gameStatus = true;

        // case : user nb is greater
        } else if (this.userNumber < this.numberToGuess) {
          this.message = `The number to guess is greater than ${this.userNumber}`;

          this.numberOfTry = this.numberOfTry - 1;

        // case : user nb is smaller
        } else {
          this.message = `The number to guess is smaller than ${this.userNumber}`;

          this.numberOfTry = this.numberOfTry - 1;
        }

        // case : out of tries
        if (this.numberOfTry < 0) {
          this.message = "You lost ! Try again";
          this.gameStatus = true; // game over
        }
      }
    },

    playAgain() {
      // Reset everything
      this.numberToGuess = null;
      this.userNumber = null;
      this.numberOfTry = 10;
      this.gameStatus = false;
      this.message = "";

      // trigger the initializedNumberToGuess function to get another random number
      this.initializedNumberToGuess();
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.title {
  margin: 4rem 0;
}

.label {
  margin-right: 0.3rem;
}

.input,.btn {
  padding: 0.3rem;
}

.btn {
  margin-left: 0.3rem;
  background-color: #5e0f5a;
  border: 2px solid #5e0f5a;
  border-radius: 3px;
  color: #fff;
  font-weight: bold;
}

.btn:hover {
  background-color: #fff;
  border: 2px solid #5e0f5a;
  color: #5e0f5a;
  cursor: pointer;
}

.nbOfTriesLeft {
  margin-top: 4rem;
}

.nbOfTriesLeft > span {
  font-weight: bold;
}
</style>
