<template>
  <div class="products-page">
    <div class="page-header">
      <p class="page-label">Portfolio</p>
      <h1 class="page-title">Product Islands</h1>
      <p class="page-sub">Systems built across the world</p>
    </div>

    <div class="products-grid">
      <div
        v-for="product in products"
        :key="product.id"
        class="product-card"
        @mouseenter="activeCarousel[product.id] = 0"
      >
        <div class="card-image">
          <img
            :src="product.images[carouselIndex[product.id] ?? 0]"
            :alt="product.title"
          />
          <div class="carousel-dots" v-if="product.images.length > 1">
            <span
              v-for="(_, i) in product.images"
              :key="i"
              :class="['dot', { active: (carouselIndex[product.id] ?? 0) === i }]"
              @click.stop="carouselIndex[product.id] = i"
            />
          </div>
          <div class="card-glow"></div>
        </div>
        <div class="card-body">
          <div class="card-tags">
            <span v-for="tag in product.tags" :key="tag" class="tag">{{ tag }}</span>
          </div>
          <h3 class="card-title">{{ product.title }}</h3>
          <p class="card-desc">{{ product.description }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive } from 'vue'
import { products } from '../data/products.js'

const carouselIndex = reactive({})
const activeCarousel = reactive({})
</script>

<style scoped>
.products-page {
  position: fixed;
  inset: 0;
  background: #050a12;
  overflow-y: auto;
  padding: 100px 48px 60px;
}

.page-header {
  text-align: center;
  margin-bottom: 56px;
}

.page-label {
  font-size: 11px;
  letter-spacing: 3px;
  text-transform: uppercase;
  color: var(--cyan);
  margin-bottom: 8px;
}

.page-title {
  font-size: 48px;
  font-weight: 700;
  color: #fff;
  margin-bottom: 10px;
}

.page-sub {
  font-size: 16px;
  color: var(--text-muted);
}

.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: 28px;
  max-width: 1200px;
  margin: 0 auto;
}

.product-card {
  background: var(--glass-bg);
  border: 1px solid var(--glass-border);
  border-radius: 20px;
  overflow: hidden;
  transition: transform 0.3s ease, border-color 0.3s, box-shadow 0.3s;
  cursor: pointer;
}

.product-card:hover {
  transform: translateY(-6px);
  border-color: var(--cyan-glow);
  box-shadow: 0 0 40px rgba(0, 255, 200, 0.12), 0 20px 60px rgba(0, 0, 0, 0.5);
}

.card-image {
  position: relative;
  height: 200px;
  overflow: hidden;
}

.card-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.product-card:hover .card-image img {
  transform: scale(1.05);
}

.card-glow {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(5, 10, 18, 0.8) 0%, transparent 60%);
}

.carousel-dots {
  position: absolute;
  bottom: 10px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 6px;
  z-index: 2;
}

.carousel-dots .dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.3);
  cursor: pointer;
  transition: background 0.2s;
}

.carousel-dots .dot.active {
  background: var(--cyan);
  box-shadow: 0 0 6px var(--cyan);
}

.card-body {
  padding: 20px;
}

.card-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin-bottom: 10px;
}

.tag {
  font-size: 10px;
  font-weight: 600;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: var(--cyan);
  background: var(--cyan-dim);
  border: 1px solid rgba(0, 255, 200, 0.2);
  padding: 3px 10px;
  border-radius: 20px;
}

.card-title {
  font-size: 18px;
  font-weight: 700;
  color: #fff;
  margin-bottom: 8px;
}

.card-desc {
  font-size: 13px;
  line-height: 1.6;
  color: var(--text-muted);
}
</style>
