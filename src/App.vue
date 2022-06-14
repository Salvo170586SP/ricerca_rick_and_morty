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
          <div
            v-else
            class="col text-center py-4"
            v-for="(character, index) in filteredCharacters"
            :key="index"
          >
            <img :src="character.image" class="shadow" alt="" />
            <h2>{{ character.name }}</h2>
            <div class="d-flex justify-content-center py-2">
              <div class="linea"></div>
            </div>
            <p>Specie: {{ character.species }}</p>
            <p >Location: {{ character.location.name }}</p>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    Header,
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
input{
      border: 0;
    border-radius: 10px;
    padding: 10px;
    width: 300px;
    outline: 0;
    transition: 0.5s ease;
}
input:focus-visible{
      box-shadow: black 0px 0px 10px -4px;

}

img {
  border-radius: 50%;
}

.linea{
  border-bottom: 2px solid black;
  width: 150px;
}
</style>
