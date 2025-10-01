
<template>
  <div class="p-3">
    <div class="row g-3">
      <div
        v-for="card in cards"
        :key="card.id"
        class="col-12 col-sm-6 col-lg-3 d-flex justify-content-center"
      >
        <!-- slot para mostrar la tarjeta completa -->
        <slot name="card" :card="card">
          <div
            class="digimon-card"
            :class="{ flipped: card.flipped }"
            @click="card.flipped = !card.flipped"
          >
            <!-- Front -->
            <div class="card-face card-front">
              <img :src="card.image" :alt="card.name" class="card-image" />
              <div class="card-info">
                <h5>{{ card.name }}</h5>
                <p>Level: {{ card.level }}</p>
                <p>Type: {{ card.type }}</p>
              </div>
            </div>

            <!-- Back -->
            <div class="card-face card-back">
              <img :src="card.tcg" :alt="card.name + ' TCG'" class="tcg-image" />
            </div>
          </div>
        </slot>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps({
  cards: {
    type: Array,
    default: () => [],
  },
});
</script>

<style scoped>
.digimon-card {
  perspective: 1000px;
  width: 100%;
  max-width: 220px; /* límite de ancho */
  height: 400px;
  position: relative;
  cursor: pointer;
  border: 2px solid #ccc;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0,0,0,0.2);
  background: #fff;
}

.card-face {
  width: 100%;
  height: 100%;
  border-radius: 10px;
  backface-visibility: hidden;
  position: absolute;
  top: 0;
  left: 0;
  overflow: hidden;
  transition: transform 0.6s;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.card-front {
  transform: rotateY(0deg);
  z-index: 2;
}

.card-back {
  transform: rotateY(180deg);
  background: #f3f3f3;
}

.digimon-card.flipped .card-front {
  transform: rotateY(-180deg);
}

.digimon-card.flipped .card-back {
  transform: rotateY(0deg);
}

.card-image {
  width: 100%;
  height: 60%;
  object-fit: contain;
}

.card-info {
  padding: 0.5rem;
  font-size: 0.85rem;
  text-align: center;
}

.tcg-image {
  width: 100%;
  height: 100%;
  object-fit: contain;
}
</style>