<template>
  <div class="home">
    <div id="app">
      <div class="modal" v-if="showModal">
        <div class="modal-content">
          <h2>Welcome to Memory Game</h2>
          <input type="text" placeholder="Enter Player 1's Name" v-model="player1Name">
          <input type="text" placeholder="Enter Player 2's Name" v-model="player2Name">
          <button @click="startGame">Start Game</button>
        </div>
      </div>
      <div class="scoreboard" v-if="!showModal">
        <h2>Scoreboard</h2>
        <p>{{ player1Name }}: {{ player1Score }}</p>
        <p>{{ player2Name }}: {{ player2Score }}</p>
      </div>
      <div class="game-board" v-if="!showModal">
        <div class="card" v-for="(card, index) in shuffledCards" :key="index" @click="flipCard(index)">
          <i v-if="!card.flipped" class="fas fa-question"></i>
          <i v-else class="fas" :class="card.icon"></i>
        </div>
      </div>
    </div>
  </div>
</template>

 

<script>
export default {
  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

<script>
export default {
  name: 'HomeView',
  data() {
    return {
      showModal: true,
      player1Name: '',
      player2Name: '',
      player1Score: 0,
      player2Score: 0,
      cards: [
        { icon: 'fa-anchor', flipped: false, matched: false },
        { icon: 'fa-anchor', flipped: false, matched: false },
        // Add more cards for the rest of the deck
      ],
      flippedCards: [],
      canFlip: true
    };
  },
  computed: {
    shuffledCards() {
      return this.cards.sort(() => Math.random() - 0.5);
    }
  },
  methods: {
    startGame() {
      this.showModal = false;
    },
    flipCard(index) {
      if (!this.canFlip || this.cards[index].flipped || this.flippedCards.length >= 2) return;

      this.cards[index].flipped = true;
      this.flippedCards.push(index);

      if (this.flippedCards.length === 2) {
        this.canFlip = false;
        setTimeout(() => {
          this.checkMatch();
          this.flippedCards = [];
          this.canFlip = true;
        }, 1000);
      }
    },
    checkMatch() {
      const card1 = this.cards[this.flippedCards[0]];
      const card2 = this.cards[this.flippedCards[1]];

      if (card1.icon === card2.icon) {
        card1.matched = true;
        card2.matched = true;
        if (this.player1Turn) {
          this.player1Score++;
        } else {
          this.player2Score++;
        }
      } else {
        card1.flipped = false;
        card2.flipped = false;
      }
    }
  }
};
</script>

<style>
/* Add your CSS styling here */
</style>

