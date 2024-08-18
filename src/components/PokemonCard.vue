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
  water: { backgroundImage: 'url(/images/water.png)' },
  fire: { backgroundImage: 'url(/images/fire.png)' },
  grass: { backgroundImage: 'url(/images/grass.png)' },
  electric: { backgroundImage: 'url(/images/electric.png)' },
  psychic: { backgroundImage: 'url(/images/psychic.png)' },
  ice: { backgroundImage: 'url(/images/ice.png)' },
  dragon: { backgroundImage: 'url(/images/dragon.png)' },
  steel: { backgroundImage: 'url(/images/steel.png)' },
  fighting: { backgroundImage: 'url(/images/fighting.png)' },
  poison: { backgroundImage: 'url(/images/poison.png)' },
  flying: { backgroundImage: 'url(/images/flying.png)' },
  bug: { backgroundImage: 'url(/images/bug.png)' },
  ground: { backgroundImage: 'url(/images/ground.png)' },
  fairy: { backgroundImage: 'url(/images/fairy.png)' },
  ghost: { backgroundImage: 'url(/images/ghost.png)' },
  rock: { backgroundImage: 'url(/images/rock.png)' },
  normal: { backgroundImage: 'url(/images/normal.png)' },
  
  
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