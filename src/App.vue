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
          <div class="col-12">
            <nav aria-label="Page navigation example">
              <ul class="pagination justify-content-center">
                <li class="page-item" :class="{ disabled: !this.pages.prev }">
                  <span class="page-link" role="button" @click="getPage('prev')"
                    >Previous</span
                  >
                </li>
                <li class="page-item" :class="{ disabled: !this.pages.next }">
                  <span class="page-link" role="button" @click="getPage('next')"
                    >Next</span
                  >
                </li>
              </ul>
            </nav>
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
      pages: { prev: null, next: null },
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
    getPage(dir) {
      if (!this.pages[dir]) return;
      this.getCharacters(this.pages[dir]);
    },
    getCharacters(url) {
      axios
        .get(url)
        .then((res) => {
          this.characters = res.data.results;

          const { next, prev } = res.data.info;
          this.pages = { prev, next };
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
    this.getCharacters("https://rickandmortyapi.com/api/character/");
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
