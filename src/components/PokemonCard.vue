<template>
  <div :class="['pokemon', pokemon.types[0].toLowerCase()]" 
  :style="cardStyle">
    <p class="pokemon-id-back">#{{ pokemon.id }}</p>
    <div class="pokemon-imagen">
      <img :src="pokemon.image" :alt="pokemon.name">
    </div>
    <div class="Pokemon-info">
      <div class="nombre-contenedor">
        <p class="pokemon-id">#{{ pokemon.id }}</p>
        <h2 class="pokemon-nombre">{{ pokemon.name }}</h2>
      </div>
      <div class="pokemon-tipos">
        <p v-for="tipo in pokemon.types" :key="tipo" :class="[tipo, 'tipo']">
          {{ tipo }}
        </p>
      </div>
      <div class="pokemon-stats">
        <p class="stat">{{ pokemon.height }}m</p>
        <p class="stat">{{ pokemon.weight }}kg</p>
      </div>
    </div>
  </div>
</template>

<script>
const typeEffects = {
  water: { backgroundImage: 'url(/pokedex-vue/images/water.png)' },
  fire: { backgroundImage: 'url(/pokedex-vue/images/fire.png)' },
  grass: { backgroundImage: 'url(/pokedex-vue/images/grass.png)' },
  electric: { backgroundImage: 'url(/pokedex-vue/images/electric.png)' },
  psychic: { backgroundImage: 'url(/pokedex-vue/images/psychic.png)' },
  ice: { backgroundImage: 'url(/pokedex-vue/images/ice.png)' },
  dragon: { backgroundImage: 'url(/pokedex-vue/images/dragon.png)' },
  steel: { backgroundImage: 'url(/pokedex-vue/images/steel.png)' },
  fighting: { backgroundImage: 'url(/pokedex-vue/images/fighting.png)' },
  poison: { backgroundImage: 'url(/pokedex-vue/images/poison.png)' },
  flying: { backgroundImage: 'url(/pokedex-vue/images/flying.png)' },
  bug: { backgroundImage: 'url(/pokedex-vue/images/bug.png)' },
  ground: { backgroundImage: 'url(/pokedex-vue/images/ground.png)' },
  fairy: { backgroundImage: 'url(/pokedex-vue/images/fairy.png)' },
  ghost: { backgroundImage: 'url(/pokedex-vue/images/ghost.png)' },
  rock: { backgroundImage: 'url(/pokedex-vue/images/rock.png)' },
  normal: { backgroundImage: 'url(/pokedex-vue/images/normal.png)' },
  
  
};
export default {
  props: {
    pokemon: {
      type: Object,
      required: true
    }
  },
  computed: {
    cardStyle() {
      const type = this.pokemon.types[0].toLowerCase();
      return typeEffects[type] || {};
    }
  }
};
</script>