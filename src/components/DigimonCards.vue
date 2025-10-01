<template>
  <div class="digimon-cards-container">
    <div v-if="loading" class="loading">Cargando Digimons...</div>
    <div v-if="error" class="error">{{ error }}</div>

    <div class="cards-grid">
      <div
        v-for="card in cards"
        :key="card.id"
        class="digimon-card"
        :class="{ flipped: card.flipped }"
        @click="toggleFlip(card)"
      >
        <div class="card-face card-front">
          <img
            v-if="card.image"
            :src="card.image"
            :alt="card.name"
            class="card-image-front"
          />
          <div class="card-info">
            <p><strong>{{ card.name }}</strong></p>
            <p>Level: {{ card.level }}</p>
            <p>Type: {{ card.type }}</p>
          </div>
        </div>

        <div class="card-face card-back">
          <img
            v-if="card.tcg"
            :src="card.tcg"
            :alt="card.name + ' tcg'"
            class="card-image-back"
          />
          <div v-else class="card-placeholder">No TCG Image</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

const cards = ref([]);
const loading = ref(false);
const error = ref(null);

const API_URL = "http://localhost:4000/products";

function toggleFlip(card) {
  card.flipped = !card.flipped;
}

async function loadDigimons() {
  loading.value = true;
  error.value = null;
  try {
    const res = await fetch(API_URL);
    if (!res.ok) throw new Error(`HTTP ${res.status}`);
    const data = await res.json();

    cards.value = data.map(d => ({
      id: d.id,
      name: d.name,
      level: d.level,
      type: d.type,
      image: d.image,
      tcg: d.tcg,
      flipped: false
    }));
  } catch (err) {
    console.error("Error cargando Digimon:", err);
    error.value = "No se pudo cargar los Digimon: " + err.message;
  } finally {
    loading.value = false;
  }
}

onMounted(loadDigimons);
</script>

<style scoped>
.digimon-cards-container {
  padding: 1rem;
}

.cards-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr); 
  gap: 1rem;
}

.digimon-card {
  perspective: 1000px;
  cursor: pointer;
  height: 400px;
  position: relative;

  border: 2px solid #ccc; 
  border-radius: 10px;  
  box-shadow: 0 4px 10px rgba(0,0,0,0.2); 
  background: #fff;           
}

.card-face {
  width: 100%;
  height: 100%;
  border-radius: 8px;
  backface-visibility: hidden;
  position: absolute;
  top: 0;
  left: 0;
  overflow: hidden;
  transition: transform 0.6s;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.card-front {
  transform: rotateY(0deg);
  z-index: 2;
  background: #fff;
}

.card-back {
  transform: rotateY(180deg);
  background: #000; 
}

.digimon-card.flipped .card-front {
  transform: rotateY(-180deg);
}

.digimon-card.flipped .card-back {
  transform: rotateY(0deg);
}


.card-image-front {
  width: 100%;
  height: 65%;
  object-fit: contain;
}


.card-image-back {
  width: 100%;
  height: 100%;
  object-fit: contain; 
  background: #000;    
}

.card-info {
  width: 100%;
  height: 35%;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  font-size: 0.9rem;
  padding: 0.5rem;
  text-align: center;
}
</style>