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
  name: 'AboutView',
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
.home {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 5px;
  text-align: center;
}

.scoreboard {
  margin-top: 20px;
  text-align: center;
}

.game-board {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.card {
  width: 100px;
  height: 100px;
  margin: 10px;
  background-color: #f0f0f0;
  border-radius: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

.card i {
  font-size: 2em;
}

.card:hover {

}

.fa-question {
  color: #333;
}
</style>
