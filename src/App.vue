<script>
import Home from "./Home.vue";
import Deck from "./Deck.vue";
import NotFound from "./404.vue";
import { useTheme } from "vuetify";

const routes = {
  "/": Home,
  "/deck": Deck,
};
export default {
  setup() {
    const theme = useTheme();
    return {
      theme,
      toggleTheme: () =>
        (theme.global.name.value = theme.global.current.value.dark
          ? "light"
          : "dark"),
    };
  },
  data() {
    return {
      currentPath: window.location.hash,
    };
  },
  computed: {
    currentView() {
      return routes[this.currentPath.slice(1) || "/"] || NotFound;
    },
  },
  mounted() {
    window.addEventListener("hashchange", () => {
      this.currentPath = window.location.hash;
    });
  },
  methods: {},
};
</script>

<template>
  <main>
    <v-app>
      <v-app-bar title="My Card Game">
        <v-btn href="#/" icon="mdi-home"></v-btn>
        <v-btn href="#/deck" icon="mdi-cards"></v-btn>
        <v-btn @click="toggleTheme" icon="mdi-animation"></v-btn>
      </v-app-bar>
      <v-main>
        <component :is="currentView" />
      </v-main>
    </v-app>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}
</style>
