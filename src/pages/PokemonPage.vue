<template>
  <h1 v-if="!pokemon">Espere porfavor ...</h1>
  <div v-else>
    <h1>¿Quien es este pokemón?</h1>
    <PokemonPicture :pokemon-id="pokemon.id" :show-pokemon="showPokemon" />
    <PokemonOptions :pokemons="pokemonsArr" @selection="checkAnswer" />
    <template v-if="showAnswer">
      <h2>{{ message }}</h2>
      <button @click="newGame">Nuevo Juego</button>
    </template>
  </div>
</template>

<script>
import PokemonPicture from "@/components/PokemonPicture";
import PokemonOptions from "@/components/PokemonOptions";
import getPokemonOptions from "@/helpers/getPokemonOptions.js";

export default {
  name: "PokemonPage",
  components: { PokemonPicture, PokemonOptions },
  data() {
    return {
      pokemonsArr: [],
      pokemon: null,
      showPokemon: false,
      showAnswer: false,
      message: "",
    };
  },
  methods: {
    async mixPokemonsArr() {
      this.pokemonsArr = await getPokemonOptions();
      const rdnInt = Math.floor(Math.random() * 4);
      this.pokemon = this.pokemonsArr[rdnInt];
    },
    checkAnswer(selectedPokemonId) {
      this.showPokemon = true;
      this.showAnswer = true;
      if (this.pokemon.id == selectedPokemonId) {
        this.message = `Correcto, es ${this.pokemon.name}`;
      } else {
        this.message = `Opps era, ${this.pokemon.name}`;
      }
    },
    newGame() {
      this.pokemon = null;
      this.showPokemon = false;
      this.showAnswer = false;
      this.message = "";
      this.mixPokemonsArr();
    },
  },
  mounted() {
    this.mixPokemonsArr();
  },
};
</script>

<style></style>
