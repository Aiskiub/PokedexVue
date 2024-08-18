<template>
    <div v-if="pokemon" class="pokemon-detail">
      <h2>{{ pokemon.name }}</h2>
      <img :src="pokemon.gifUrl" :alt="pokemon.name" class="pokemon-gif" />
      <p><strong>ID:</strong> #{{ pokemon.id }}</p>
      <p><strong>Height:</strong> {{ pokemon.height / 10 }}m</p>
      <p><strong>Weight:</strong> {{ pokemon.weight / 10 }}kg</p>
      
      <div class="container">
        <div v-for="stat in pokemon.stats" :key="stat.name" class="skill-box">
          <span class="title">{{ stat.name.toUpperCase() }}</span>
          <div class="skill-bar">
            <span class="skill-per" :style="{ width: `${calculatePercentage(stat.value)}%` }">
              <span class="tooltip">{{ stat.value }}</span>
            </span>
          </div>
        </div>
      </div>
  
      <h3>Types:</h3>
      <ul>
        <li v-for="type in pokemon.types" :key="type" :class="type.toLowerCase()">
          {{ type }}
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      pokemon: {
        type: Object,
        required: true,
      },
    },
    methods: {
      calculatePercentage(value) {
        // El máximo valor de estadística en Pokémon es generalmente 255
        const maxStat = 255;
        return Math.min(Math.round((value / maxStat) * 100), 100);
      }
    }
  };
  </script>
  
  <style scoped>
  .container {
    position: relative;
    max-width: 500px;
    width: 100%;
    background: #ffffff;
    margin: 0 15px;
    padding: 10px 20px;
    border-radius: 7px;
  }
  
  .container .skill-box {
    width: 100%;
    margin: 25px 0;
  }
  
  .skill-box .title {
    font-size: 14px;
    font-weight: 500;
    color: #333;
    margin-bottom: 10px;
    display: block;
  }
  
  .skill-box .skill-bar {
    height: 8px;
    width: 100%;
    border-radius: 6px;
    margin-top: 6px;
    background: rgba(0, 0, 0, 0.1);
  }
  
  .skill-bar .skill-per {
    position: relative;
    display: block;
    height: 100%;
    border-radius: 6px;
    background: #4070f4;
    animation: progress 0.4s ease-in-out forwards;
    opacity: 0;
  }
  
  @keyframes progress {
    0% {
      width: 0;
      opacity: 0;
    }
    100% {
      opacity: 1;
    }
  }
  
  .skill-bar .tooltip {
    position: absolute;
    right: -14px;
    top: -28px;
    font-size: 9px;
    font-weight: 500;
    color: #ffff;
    padding: 2px 6px;
    border-radius: 3px;
    background: #4070f4;
    z-index: 1;
  }
  
  .tooltip::before {
    content: "";
    position: absolute;
    left: 50%;
    bottom: -2px;
    height: 10px;
    width: 10px;
    z-index: -1;
    background: #4070f4;
    transform: translateX(-50%) rotate(45deg);
  }

.skill-bar .tooltip{
    position: absolute;
    right: -14px;
    top: -28px;
    font-size: 9px;
    font-weight: 500;
    color: #ffff;
    padding: 2px 6px;
    border-radius: 3px;
    background: #4070f4;
    z-index: 1;

}

.tooltip::before {
    content: "";
    position: absolute;
    bottom: 2px;
    height: 10px;
    width: 10px;
    z-index: -1;
    background: #4070f4;
    transform: translateX(-50%) rotate(45deg);
}


.pokemon-detail {
  background-color: #f0f0f0;
  border-radius: 10px;
  padding: 20px;
  text-align: center;
}

.pokemon-gif {
  max-width: 400px;
  height: auto;
  max-height: 300px;
  margin-bottom: 15px;
  width: 100px;
  image-rendering: pixelated;
}

.pokemon-detail ul {
  list-style-type: none;
  padding: 0;
  display: flex;
  justify-content: center;
  gap: 10px;
}

.pokemon-detail li {
  padding: 5px 10px;
  border-radius: 5px;
  color: white;
  font-weight: bold;
}

/* Estilos para los tipos de Pokémon (puedes ajustarlos según tus preferencias) */
.normal {
  background-color: #a8a878;
}
.fire {
  background-color: #f08030;
}
.water {
  background-color: #6890f0;
}
/* ... (añade estilos para los demás tipos) ... */
</style>
