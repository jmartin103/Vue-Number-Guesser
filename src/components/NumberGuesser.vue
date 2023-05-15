<template>
  <main>
    <div v-if="!gameInProgress">
      <div>
        <h1>Welcome to Vue Number Guesser!</h1>
      </div>
      <div>
        <h2>Select level of difficulty: </h2>
      </div>
      <div>
        <button @click="setDifficulty('Easy')">Easy</button>
        <button @click="setDifficulty('Hard')">Hard</button>
      </div>
    </div>
    <div v-else>
      <div>
        <h1>I'm thinking of a number between 1 and 100!</h1>
      </div>
      <div class="container">
        <label for="numberGuessTxt">Guess the number I'm thinking of: </label>
        <input type="text" v-model="playerGuess">
        <button @click="guess">Guess</button>
      </div>
      <div class="container">
        <span>Number of guesses remaining: {{ numberOfGuesses }}</span>
      </div>
      <div>
        <h4>Guesses: </h4>
        <ul v-for="g in guesses" :key="g.id">
          {{ g.value }} - <span>{{ g.isTooLow ? 'Too Low' : 'Too High' }}</span>
        </ul>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  computed: {
    outOfGuesses() {
      return this.numberOfGuesses < 1
    }
  },
  data() {
    return {
      difficulty: '',
      gameInProgress: false,
      playerGuess: '',
      correctGuess: Math.floor(Math.random() * 100) + 1,
      numberOfGuesses: 10,
      guesses: []
    }
  },
  methods: {
    setDifficulty(difficulty) {
      if (difficulty === 'Easy') {
        this.numberOfGuesses = 10
      } else {
        this.numberOfGuesses = 5
      }
      this.gameInProgress = true
    },
    decrementGuesses() {
      this.numberOfGuesses = this.numberOfGuesses - 1
      this.playerGuess = ''
    },
    alreadyGuessed(value) {
      return this.guesses.find((guess) => guess.value === value)
    },
    resetAll() {
      this.playerGuess = ''
      this.numberOfGuesses = 10
      this.guesses = []
      this.correctGuess = Math.floor(Math.random() * 100) + 1
    },
    guess() {
      if (this.numberOfGuesses > 1) {
        if (this.playerGuess < 1 || this.playerGuess > 100) {
          alert('Invalid guess! Number must be between 1 and 100!')
          this.playerGuess = ''
        } else if (this.alreadyGuessed(this.playerGuess)) {
          alert('You already guessed this number! Please guess a different number!')
          this.playerGuess = ''
        } else if (this.playerGuess < this.correctGuess) {
          alert('Guess is too low! Try again!')
          this.guesses.push({
            value: this.playerGuess,
            isTooLow: true,
            isTooHigh: false
          })
          this.numberOfGuesses = this.numberOfGuesses - 1
          this.playerGuess = ''
        } else if (this.playerGuess > this.correctGuess) {
          alert('Guess is too high! Try again!')
          this.guesses.push({
            value: this.playerGuess,
            isTooLow: false,
            isTooHigh: true
          })
          this.numberOfGuesses = this.numberOfGuesses - 1
          this.playerGuess = ''
        } else {
          alert('You guessed the correct number! Way to go!')
          this.resetAll()
          this.gameInProgress = false
        } 
      } else {
        alert(`You're out of guesses! The number I was thinking of was ${ this.correctGuess }!`)
        this.resetAll()
        this.gameInProgress = false
      }
    }
  }
}
</script>