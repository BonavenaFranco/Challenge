<template>
  <div class="container">
  <header class="header">
    <label for="pokemonInput">
      Type pokemon name or ID:
      <input
        type="text"
        id="pokemonInput"
        v-model="pokemonID"
      >
    </label>
    <button
      class="searchButton"
      @click="searchPokemon(false)"
    >
      Search pokemon!
    </button>
  </header>

  <main
    class="main"
    v-if="Object.entries(pokemonData).length > 0"
  >
    <section class="pokemonCard">
      <div :class="backgroundcirculeClass">
      <div class="nameImage" >
        <h1 class="pokemonName">{{ pokemonData.name }}</h1>
        <img
          :src="pokemonData.sprites.front_default"
          :alt="pokemonData.name"
        >
      </div>
    </div>
      <ul class="type">
        <li
          v-for="(type, key) in pokemonData.types"
          :key="key"
          :class="type.type.name"
        >
          <span>{{ type.type.name }}</span>
        </li>
      </ul>
      <div class="statscontainer">
        <h2 class="titlestats">Stats:</h2>
        <ul class="stats">
          <li
          v-for="(stat, key) in pokemonData.stats"
          :key="key"
          >
          <span :style="cssProps">{{ stat.stat.name}}: </span>
          <span class="statValue">{{stat.base_stat }}</span>
          <!-- <span :style="width: 25%"></span> -->
          <div class="barra"> 
            <!-- <span :style="{ width: stat.base_stat/2 + '%' }" :class="backgroundcirculeClass"></span> -->
            <span :style="progessFrame(stat)" :class="backgroundcirculeClass"></span>
          </div>
        </li>
      </ul>
    </div>
    </section>
  </main>
</div>
</template>

<script>
import { pokeapi } from '@/api/pokeapi'

export default {
  name: 'App',
  data () {
    return {
      pokemonData: {},
      pokemonID: '',
      colorType: null
    }
  },
  computed: {
    backgroundcirculeClass() { 
      const color = this.colorType;
      return ['backgroundcircule', `poke-color-${color}`];
    },
    
    cssProps() {
      const color = `$${this.colorType}`
      return { 'background-color': `${color}` };
    },
  },
  mounted() {
    this.searchPokemon(true);
  },
  methods: {
    progessFrame(stat) {
      const statValue = stat.base_stat / 2;
      return `width: ${statValue}%;
              @keyframes progress {
              0% {width: 0%}
              100% {width: ${statValue}%}};`
    },
    async searchPokemon(showPokemon) {
      if (showPokemon) {
        const pokemonToFind = await fetch(`${pokeapi}/150`)
          const pokemon = await pokemonToFind.json()
          this.pokemonData = pokemon
          this.colorType = pokemon.types[0].type.name
          return pokemon
      } else {
        try {
          const pokemonToFind = await fetch(`${pokeapi}/${this.pokemonID}`)
          const pokemon = await pokemonToFind.json()
          this.pokemonData = pokemon
          this.colorType = pokemon.types[0].type.name
          return pokemon
        } catch (error) {
          alert('Pokemon was not found')
        }  
      }
      
      
    },
  }
}
</script>

<style lang="scss" scoped>
@import './pokemon_types.scss';
@import url('https://fonts.googleapis.com/css2?family=Changa:wght@400;700&display=swap');

.header, .main, input[type="text"], .searchButton {
  font-family: 'Changa', sans-serif;
  margin:0;
  padding: 0;
}
.header, input[type="text"], .searchButton {
  font-size: 1.5rem;
}

.main {
  font-size: 1.2rem;
  margin:0px;
  padding: 0px;
  display: flex;
  align-items: center;
  justify-content: center;
 
}
.backgroundcircule {
  // top: 94px;
    width: 458px;
    height: 196px;
    position: relative;
    border-radius: 0 0 100% 100%;
    &.poke-color-normal {
  background-color: $normal;
}
 &.poke-color-fire {
  background-color: $fire;
}
 &.poke-color-water {
  background-color: $water;
}
 &.poke-color-grass {
  background-color: $grass;
}
 &.poke-color-electric {
  background-color: $electric;
}
 &.poke-color-ice {
  background-color: $ice;
 }
 &.poke-color-fighting {
  background-color: $fighting;
}
 &.poke-color-poison {
  background-color: $poison;
}
 &.poke-color-ground {
  background-color: $ground;
}
 &.poke-color-flying {
  background-color: $flying;
}
 &.poke-color-psychic {
  background-color: $psychic;
}
 &.poke-color-bug {
  background-color: $bug;
}
 &.poke-color-rock {
  background-color: $rock;
}
 &.poke-color-ghost {
  background-color: $ghost;
}
 &.poke-color-dark {
  background-color: $dark;
}
 &.poke-color-dragon {
  background-color: $dragon;
}
 &.poke-color-steel {
  background-color: $steel;
}
 &.poke-color-fairy {
  background-color: $fairy;
}

}
.header {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 15vh;
  gap: 1rem;
  background-color: $pokedex-red;
  color: white;

  & .searchButton {
  background-color: #4a0eaa;
  color: white;
  border: none;
  margin-left: 10px;
  border-radius: 10px;
  cursor: pointer;

  &:hover {
    background-color: #7f14c2;
  }
}

  & input[type="text"] {
    border-radius: 10px;
    outline: none;
    border: none;
  }
}


.pokemonCard {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-top: 22px;
    overflow: hidden;
    // background-color: $pokedex-green;
    width: 24vw;
    // height: 740px;
    // border: 10px solid red;
    padding: 20px;
    padding-top: 0;
  border-radius: 10px;
  box-shadow: 4px 4px 10px #000;


  & .nameImage, & .stats {
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
  }
  .type{
    display: flex;
    flex-direction: row;
    width: 8vw;
    margin-top: 5rem;
    margin-bottom: 0px;
    gap: 0.5rem;
  }

  & .nameImage {
    z-index: 100;
    & .pokemonName {
      text-transform: capitalize;
      margin: 0;
    }

    & img {
      width: 200px;
      // background-color: $pokedex-blue;
      border-radius: 20%;
    }
  }

  & .type li {
    width: 90%;
    margin-bottom: 10px;
    text-align: center;
    border-radius: 20px;
  }
  & .statscontainer{
    display: flex;
    flex-direction: column;
    width: 100%;
    & .titlestats{
      margin: 0;
    }
    & .stats {
      display: flex;
      widows: 100%;
      flex-direction: row;
      align-self: flex-start;
      flex-wrap: wrap;
      width: auto;
      gap: 1rem;

      li{
        width: 100%;
      }
    }

  }
}

ul {
  padding: 0;
}

.type {
  & li {
    list-style: none;
    color: white;
    text-transform: uppercase;
  }
}

.stats {
  color: black;

  & li {
    list-style: none;
    text-transform: uppercase;
  }
}

.normal {
  background-color: $normal
}
.fire {
  background-color: $fire
}
.water {
  background-color: $water
}
.grass {
  background-color: $grass
}
.electric {
  background-color: $electric
}
.ice {
  background-color: $ice
}
.fighting {
  background-color: $fighting
}
.poison {
  background-color: $poison
}
.ground {
  background-color: $ground
}
.flying {
  background-color: $flying
}
.psychic {
  background-color: $psychic
}
.bug {
  background-color: $bug
}
.rock {
  background-color: $rock
}
.ghost {
  background-color: $ghost
}
.dark {
  background-color: $dark
}
.dragon {
  background-color: $dragon
}
.steel {
  background-color: $steel
}
.fairy {
  background-color: $fairy
}

@media screen and (max-width: 820px) {

  .header {
    flex-direction: column;
    height: 120px;

    & .searchButton {
      width: 70%;
      margin-top: 10px;
    }
  }
  .pokemonCard {
    flex-direction: column;
    align-items: center;
  }
}

@media screen and (max-width: 600px) {
  .header {
    font-size: 1rem;

    & input[type="text"] {
      font-size: 1rem;
    }

    & .searchButton {
      font-size: 1rem;
    }
  }

  .pokemonCard {
    & .stats {
      width: 90%;
    }
  }
}
.barra { 
    height: 20px;
	position: relative;
	// background: #555;
	background: rgb(12, 32, 32);
	border-radius: 25px;
	padding: 10px;
}
.barra > span {
  display: block;
  height: 100%;
  border-top-right-radius: 8px;
  border-bottom-right-radius: 8px;
  border-top-left-radius: 20px;
  border-bottom-left-radius: 20px;
  background-color: #22AA11;
  position: relative;
  overflow: hidden;
  animation: progress 3s linear;
}

@media screen and (max-width: 400px) {
  .header {
    & label {
      text-align: center;
    }
  }
}
</style>