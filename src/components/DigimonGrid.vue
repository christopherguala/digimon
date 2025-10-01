
<template>
  <div class="p-3 border rounded bg-light">
    <div class="d-flex flex-wrap justify-content-start">
      <div
        v-for="card in cards"
        :key="card.name + card.tcgImage || Math.random()"
        class="p-2"
        style="width: calc(20% - 16px);" 
      >
        <slot name="card" :card="card">
          <div class="card h-100">
            <img :src="card.frontImg" class="card-img-top" :alt="card.name" />
            <div class="card-body">
              <h5 class="card-title">{{ card.name }}</h5>
              <p class="card-text">Level: {{ card.level }}</p>
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
</style>