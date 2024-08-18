<template>
  <div>
    <header>
      <nav class="nav">
        <img src="/images/logo.png" alt="Logo Pokédex" />
        <ul class="nav-list">
          <button
            v-for="tipo in tipos"
            :key="tipo.id"
            :id="tipo.id"
            :class="['btn', 'btn-header', tipo.class]"
            @click="filtrarPokemon(tipo.id)"
          >
            {{ tipo.text }}
          </button>
        </ul>
      </nav>
    </header>
    <main class="main-content">
      <div class="pokemon-grid" :class="{ 'with-detail': selectedPokemon }">
        <PokemonCard
          v-for="pokemon in pokemonFiltrados"
          :key="pokemon.id"
          :pokemon="pokemon"
          @click="selectPokemon(pokemon)"
        />
      </div>
      <transition name="slide-fade">
        <PokemonDetail 
          v-if="selectedPokemon" 
          :pokemon="selectedPokemon" 
          class="pokemon-detail" 
          @close="closeDetail"
        />
      </transition>
    </main>
  </div>
</template>

<script>
import PokemonCard from "./PokemonCard.vue";
import PokemonDetail from "./PokemonDetail.vue";

export default {
  components: {
    PokemonCard,
    PokemonDetail
  },
  data() {
    return {
      pokemonList: [],
      pokemonFiltrados: [],
      tipos: [
        { text: "Ver Todos", id: "ver-todos" },
        { text: "Normal", id: "normal", class: "normal" },
        { text: "Fire ", id: "fire", class: "fire" },
        { text: "Water", id: "water", class: "water" },
        { text: "Grass", id: "grass", class: "grass" },
        { text: "Electric", id: "electric", class: "electric" },
        { text: "Ice", id: "ice", class: "ice" },
        { text: "Fighting", id: "fighting", class: "fighting" },
        { text: "Poison", id: "poison", class: "poison" },
        { text: "Ground", id: "ground", class: "ground" },
        { text: "Flying", id: "flying", class: "flying" },
        { text: "Psychic", id: "psychic", class: "psychic" },
        { text: "Bug", id: "bug", class: "bug" },
        { text: "Rock", id: "rock", class: "rock" },
        { text: "Ghost", id: "ghost", class: "ghost" },
        { text: "Dark", id: "dark", class: "dark" },
        { text: "Dragon", id: "dragon", class: "dragon" },
        { text: "Steel", id: "steel", class: "steel" },
        { text: "Fairy", id: "fairy", class: "fairy" },
      ],
      URL: "https://pokeapi.co/api/v2/pokemon/",
      selectedPokemon: null
    };
  },
  mounted() {
    this.cargarPokemon();
  },
  methods: {
    async cargarPokemon() {
      for (let i = 1; i <= 151; i++) {
        try {
          const response = await fetch(this.URL + i);
          const data = await response.json();
          this.pokemonList.push(this.formatearPokemon(data));
        } catch (error) {
          console.error("Error cargando Pokémon:", error);
        }
      }
      this.pokemonFiltrados = this.pokemonList;
    },
    formatearPokemon(poke) {
      let pokeID = poke.id.toString().padStart(3, "0");
      return {
        id: pokeID,
        name: poke.name,
        image: poke.sprites.other["official-artwork"].front_default,
        gifUrl: poke.sprites.versions["generation-v"]["black-white"].animated.front_default || poke.sprites.front_default,
        types: poke.types.map((type) => type.type.name),
        height: poke.height,
        weight: poke.weight,
      };
    },
    filtrarPokemon(tipo) {
      if (tipo === "ver-todos") {
        this.pokemonFiltrados = this.pokemonList;
      } else {
        this.pokemonFiltrados = this.pokemonList.filter((pokemon) =>
          pokemon.types.includes(tipo)
        );
      }
    },
    selectPokemon(pokemon) {
      console.log("Seleccionado:", pokemon);
      /* Borrar dos ceros al comienzo */
      const formatId = pokemon.id.replace(/^0+/, "");
      /* Crear la URL de los detalles del Pokémon */
      const detailsURL = `https://pokeapi.co/api/v2/pokemon/${formatId}`;
      console.log("URL de detalles:", detailsURL);
      /* Realizar la petición a la API */
      fetch(detailsURL)
        .then((response) => response.json())
        .then((data) => {
          console.log("Detalles del Pokémon:", data);
          /* Actualizar el Pokémon seleccionado con los detalles */
          this.selectedPokemon = {
            ...pokemon,
            abilities: data.abilities.map((ability) => ability.ability.name),
            stats: data.stats.map((stat) => ({
              name: stat.stat.name,
              value: stat.base_stat,
            })),
          };
        })
        .catch((error) => {
          console.error("Error cargando detalles del Pokémon:", error);
        });
    },
  },
};
</script>

<style scoped>
.main-content {
  display: flex;
  justify-content: space-between;
  padding: 20px;
  margin-top: 60px; 
  /* Ajusta este valor según la altura de tu header */
}

.pokemon-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  transition: all 0.3s ease;
}

.pokemon-grid.with-detail {
  width: 80%;
}

.pokemon-detail {
  width: 300px;
  background-color: var(--clr-white);
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  height: calc(100vh - 4rem);
  position: sticky;
  top: 2rem;
  overflow-y: auto;
  margin: 0 80px;
}

.slide-fade-enter-active, .slide-fade-leave-active {
  transition: all 0.3s ease;
}
.slide-fade-enter, .slide-fade-leave-to {
  transform: translateX(20px);
  opacity: 0;
}

@media (max-width: 1200px) {
  .pokemon-grid {
    grid-template-columns: repeat(1, 1fr);
  }
  .pokemon-grid.with-detail {
    grid-template-columns: repeat(2, 1fr);
    width: 50%;
  }
}

@media (max-width: 768px) {
  .main-content {
    flex-direction: column;
  }
  .pokemon-grid, .pokemon-grid.with-detail {
    width: 100%;
    grid-template-columns: repeat(1, 1fr);
  }
  .pokemon-detail {
    width: 100%;
    position: static;
    margin-top: 20px;
  }
}
</style>