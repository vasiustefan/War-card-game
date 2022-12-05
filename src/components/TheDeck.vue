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
        });
    },
  },
};
</script>

<template>
  <v-btn @click="fetchDeck"
    >Get my Deck <v-icon icon="mdi-play-box-outline"></v-icon
  ></v-btn>
  <div v-if="deckData">
    <v-btn variant="outlined" @click="fetchCard(deckData.deck_id)"
      >Draw Cards<v-icon icon="mdi-account-supervisor-outline"></v-icon
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
  </div>
</template>

<style scoped>
img {
  float: left;
  margin-right: -160px;
}
</style>
