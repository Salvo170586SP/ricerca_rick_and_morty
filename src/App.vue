<template>
  <div id="app">
    <Header />
    <main class="py-5">
      <div class="container">
        <div class="row">
          <div class="col-12 py-5 text-center">
            <input type="text" placeholder="cerca personaggio" v-model="text" />
          </div>
          <div v-if="!filteredCharacters.length" class="text-center">
            <h2>la ricerca non ha prodotto nessun risultato</h2>
          </div>
          <CardCharacters
            v-for="(character, index) in filteredCharacters"
            :key="index"
            :character="character"
          />
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import CardCharacters from "./components/CardCharacters.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    Header,
    CardCharacters,
  },
  data() {
    return {
      characters: [],
      text: "",
    };
  },
  computed: {
    filteredCharacters() {
      const search = this.text.toLowerCase();
      return this.characters.filter((character) => {
        return character.name.toLowerCase().includes(search);
      });
    },
  },
  methods: {
    getCharacters() {
      axios
        .get("https://rickandmortyapi.com/api/character/")
        .then((res) => {
          this.characters = res.data.results;
          console.log(this.characters);
        })
        .catch((err) => {
          console.error(err);
        })
        .then(() => {
          console.log("chiamata terminata");
        });
    },
  },
  mounted() {
    this.getCharacters();
  },
};
</script>

<style lang="scss">
@import "./assets/scss/app.scss";
input {
  border: 0;
  border-radius: 10px;
  padding: 10px;
  width: 300px;
  outline: 0;
  transition: 0.5s ease;
}
input:focus-visible {
  box-shadow: black 0px 0px 10px -4px;
}
</style>
