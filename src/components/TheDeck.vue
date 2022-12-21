<script>
export default {
  data() {
    return {
      deckData: null,
      cardData: null,
      deckCardsData: [],
      player1Score: 0,
      player2Score: 0,
    };
  },
  methods: {
    fetchDeck() {
      const Url =
        "https://deckofcardsapi.com/api/deck/new/shuffle/?deck_count=1";
      fetch(Url)
        .then((response) => response.json())
        .then((data) => {
          this.deckData = data;
          this.deckCards = null;
          this.deckCardsData = [];
        });
    },
    fetchCard(deckId) {
      const Url = `https://deckofcardsapi.com/api/deck/${deckId}/draw/?count=2`;
      fetch(Url)
        .then((response) => response.json())
        .then((data) => {
          this.deckCardsData = data.cards;
          this.scoreBoard();
        });
    },

    scoreBoard() {
      let data = this.deckCardsData;
      console.log(data[0]);
      console.log(data[1]);
      if (data[0].value === "10") {
        data[0].value = 10;
      }
      if (data[1].value === "10") {
        data[1].value = 10;
      }
      if (
        (data[1].value === "ACE" && data[0].value != "ACE") ||
        data[0].value < data[1].value
      ) {
        this.player2Score = ++this.player2Score;
      } else {
        if (
          (data[0].value === "ACE" && data[1].value != "ACE") ||
          data[0].value > data[1].value
        )
          this.player1Score = ++this.player1Score;
      }
      if (this.player1Score === 5) {
        alert("Player 1 wins!");
        this.player1Score = 0;
        this.player2Score = 0;
        this.fetchDeck();
      }
      if (this.player2Score === 5) {
        alert("Player 2 wins!");
        this.player1Score = 0;
        this.player2Score = 0;
        this.fetchDeck();
      }
    },
  },
};
</script>

<template>
  <v-btn @click="fetchDeck"
    >Get my Deck <v-icon icon="mdi-play-box-outline"></v-icon
  ></v-btn>
  <div v-if="deckData">
    <v-btn variant="outlined" @click="fetchCard(deckData.deck_id)">
      Draw Cards<v-icon icon="mdi-account-supervisor-outline"></v-icon
    ></v-btn>
    <div v-if="cardData">
      <v-img
        class="bg-white"
        width="100"
        :aspect-ratio="1"
        :src="cardData.cards[0].image"
      />
    </div>
    <v-row v-if="deckCardsData.length > 0">
      <v-col
        v-for="(card, index) in deckCardsData"
        :key="index"
        class="d-flex child-flex"
        cols="2"
      >
        <v-img
          class="bg-grey-lighten-2"
          cover
          :aspect-ratio="0"
          :src="card.image"
        />
      </v-col>
    </v-row>
    <div id="score">
      <h2>Player 1 Score:{{ player1Score }}</h2>
      <h2>Player 2 Score:{{ player2Score }}</h2>
    </div>
  </div>
</template>

<style scoped>
img {
  float: left;
  margin-right: -160px;
}
</style>
