<template>
  <div class="col text-center py-4">
    <img :src="character.image" class="shadow" alt="" />
    <h2>{{ character.name }}</h2>
    <div class="d-flex justify-content-center py-2">
      <div class="linea"></div>
    </div>
    <p>Specie: {{ character.species }}</p>
    <p>Location: {{ character.location.name }}</p>
  </div>
</template>

<script>
export default {
  name: "CardCharacters",
  props: ["character"],
};
</script>

<style scoped>
img {
  border-radius: 50%;
}

.linea {
  border-bottom: 2px solid black;
  width: 150px;
}
</style>