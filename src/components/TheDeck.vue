<script>
export default {
  data() {
    return {
      deckData: null,
      cardData: null,
      deckCardsData: [],
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
      const Url = `https://deckofcardsapi.com/api/deck/${deckId}/draw/?count=1`;
      fetch(Url)
        .then((response) => response.json())
        .then((data) => {
          this.cardData = data;
          this.deckCardsData = [];
        });
    },
    returnToDeck() {
      this.cardData = null;
      this.deckCardsData = [];
    },
  },
};
</script>

<template>
  <h2>Card Deck</h2>
  <v-btn variant="outlined" rounded="pill" color="white" @click="fetchDeck"
    >Get new Deck</v-btn
  >
  <div class="d-flex justify-space-between" v-if="deckData">
    <v-btn
      variant="outlined"
      rounded="pill"
      color="white"
      v-if="cardData || deckCardsData.length > 0"
      @click="returnToDeck"
    >
      Return to Deck
    </v-btn>
    <v-btn
      variant="outlined"
      rounded="pill"
      color="white"
      @click="fetchCard(deckData.deck_id)"
      >Get Card Player 1</v-btn
    >

    <v-btn
      variant="outlined"
      rounded="pill"
      color="white"
      @click="fetchCard(deckData.deck_id)"
      >Get Card Player 2</v-btn
    >
    <div v-if="cardData">
      <img :src="cardData.cards[0].image" />
    </div>
    <div v-if="deckCardsData.length > 0">
      <div v-for="(card, index) in deckCardsData" :key="index">
        <img :src="card.image" />
      </div>
    </div>
    <v-btn
      v-if="deckCardsData.length > 0"
      @click="shuffleRemainingDeck(deckData.deck_id)"
    >
      Shuffle Remaining Deck
    </v-btn>
  </div>
</template>
fetch
<style scoped>
img {
  float: left;
  margin-right: -160px;
}
</style>
