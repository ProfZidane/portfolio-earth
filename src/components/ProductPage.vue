<template>
  <div class="products-page">

    <div class="page-header">
      <p class="page-label">Portfolio</p>
      <h1 class="page-title">Product Islands</h1>
      <p class="page-sub">Systems built across the world — click to explore</p>
    </div>

    <!-- Horizontal scroll track -->
    <div
      class="scroll-track"
      ref="trackRef"
      @mousedown="startDrag"
      @mousemove="onDrag"
      @mouseup="stopDrag"
      @mouseleave="stopDrag"
    >
      <div
        v-for="product in products"
        :key="product.id"
        class="product-card"
        @click="openModal(product)"
      >
        <div class="card-image">
          <img :src="product.cover" :alt="product.title" draggable="false" />
          <div class="card-overlay"></div>
          <div class="card-tags">
            <span v-for="tag in product.tags" :key="tag" class="tag">{{ tag }}</span>
          </div>
        </div>
        <div class="card-body">
          <h3 class="card-title">{{ product.title }}</h3>
          <p class="card-tagline">{{ product.tagline }}</p>
          <span class="card-cta">Explore →</span>
        </div>
      </div>
    </div>

    <!-- Scroll hint -->
    <p class="scroll-hint">← drag or scroll to explore →</p>

    <ProductModal :product="activeProduct" @close="activeProduct = null" />
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { products } from '../data/products.js'
import ProductModal from './ProductModal.vue'

const activeProduct = ref(null)
const trackRef = ref(null)

const openModal = (product) => {
  if (!isDragging.value) activeProduct.value = product
}

// ── Wheel → horizontal scroll (passive, works for trackpad + mouse wheel) ──
const onWheel = (e) => {
  // If the event has horizontal delta (trackpad native swipe), let it pass through.
  // Only redirect vertical-only scroll (mouse wheel) to horizontal.
  if (Math.abs(e.deltaX) > Math.abs(e.deltaY)) return
  trackRef.value.scrollLeft += e.deltaY
}

onMounted(() => {
  // passive:true lets the browser handle trackpad momentum natively
  trackRef.value.addEventListener('wheel', onWheel, { passive: true })
})

onBeforeUnmount(() => {
  trackRef.value?.removeEventListener('wheel', onWheel)
})

// ── Drag to scroll ──
const isDragging = ref(false)
let dragStartX = 0
let scrollStartX = 0
let dragMoved = false

const startDrag = (e) => {
  isDragging.value = false
  dragMoved = false
  dragStartX = e.pageX
  scrollStartX = trackRef.value.scrollLeft
  trackRef.value.style.cursor = 'grabbing'
  trackRef.value.style.userSelect = 'none'
}

const onDrag = (e) => {
  if (e.buttons !== 1) return
  const delta = e.pageX - dragStartX
  if (Math.abs(delta) > 4) {
    isDragging.value = true
    dragMoved = true
  }
  if (dragMoved) trackRef.value.scrollLeft = scrollStartX - delta
}

const stopDrag = () => {
  trackRef.value.style.cursor = 'grab'
  trackRef.value.style.userSelect = ''
  setTimeout(() => { isDragging.value = false }, 0)
}
</script>

<style scoped>
.products-page {
  position: fixed;
  inset: 0;
  background: #050a12;
  display: flex;
  flex-direction: column;
  justify-content: center;
  overflow: hidden;
}

/* ── Header ── */
.page-header {
  text-align: center;
  padding-top: 90px;
  padding-bottom: 40px;
  flex-shrink: 0;
}

.page-label {
  font-size: 11px;
  letter-spacing: 3px;
  text-transform: uppercase;
  color: var(--cyan);
  margin-bottom: 8px;
}

.page-title {
  font-size: 44px;
  font-weight: 700;
  color: #fff;
  margin-bottom: 8px;
}

.page-sub {
  font-size: 15px;
  color: var(--text-muted);
}

/* ── Scroll track ── */
.scroll-track {
  display: flex;
  gap: 28px;
  padding: 20px 80px 32px;
  overflow-x: auto;
  overflow-y: visible;
  scroll-behavior: smooth;
  cursor: grab;
  flex-shrink: 0;
  scrollbar-width: none;
  /* enable native touch/trackpad horizontal scroll */
  -webkit-overflow-scrolling: touch;
  overscroll-behavior-x: contain;
}
.scroll-track::-webkit-scrollbar { display: none; }

/* ── Card ── */
.product-card {
  flex-shrink: 0;
  width: 380px;
  background: var(--glass-bg);
  border: 1px solid var(--glass-border);
  border-radius: 22px;
  overflow: hidden;
  cursor: pointer;
  transition: transform 0.35s cubic-bezier(0.34, 1.56, 0.64, 1),
              border-color 0.3s ease,
              box-shadow 0.3s ease;
  will-change: transform;
}

@media (max-width: 768px) {
  .scroll-track {
    padding: 16px 24px 24px;
    gap: 18px;
  }
  .product-card {
    width: 80vw;
    max-width: 320px;
  }
  .page-title { font-size: 30px; }
  .page-header { padding-top: 80px; padding-bottom: 24px; }
}

.product-card:hover {
  transform: translateY(-10px) scale(1.02);
  border-color: var(--cyan-glow);
  box-shadow:
    0 0 0 1px rgba(0, 255, 200, 0.15),
    0 0 50px rgba(0, 255, 200, 0.12),
    0 30px 70px rgba(0, 0, 0, 0.6);
}

/* ── Card image ── */
.card-image {
  position: relative;
  height: 240px;
  overflow: hidden;
}

.card-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.5s ease;
  pointer-events: none;
}

.product-card:hover .card-image img {
  transform: scale(1.07);
}

.card-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(
    to bottom,
    transparent 30%,
    rgba(5, 10, 18, 0.7) 80%,
    rgba(5, 10, 18, 0.95) 100%
  );
}

.card-tags {
  position: absolute;
  top: 14px;
  left: 14px;
  display: flex;
  gap: 6px;
  flex-wrap: wrap;
  z-index: 2;
}

.tag {
  font-size: 10px;
  font-weight: 600;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: var(--cyan);
  background: rgba(0, 255, 200, 0.12);
  border: 1px solid rgba(0, 255, 200, 0.25);
  padding: 3px 9px;
  border-radius: 20px;
  backdrop-filter: blur(8px);
}

/* ── Card body ── */
.card-body {
  padding: 22px 24px 26px;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.card-title {
  font-size: 20px;
  font-weight: 700;
  color: #fff;
}

.card-tagline {
  font-size: 13px;
  line-height: 1.6;
  color: var(--text-muted);
}

.card-cta {
  font-size: 12px;
  font-weight: 600;
  color: var(--cyan);
  letter-spacing: 0.5px;
  margin-top: 4px;
  opacity: 0;
  transform: translateX(-6px);
  transition: opacity 0.25s, transform 0.25s;
}

.product-card:hover .card-cta {
  opacity: 1;
  transform: translateX(0);
}

/* ── Scroll hint ── */
.scroll-hint {
  text-align: center;
  font-size: 11px;
  letter-spacing: 2px;
  color: rgba(232, 234, 240, 0.2);
  padding-bottom: 24px;
  flex-shrink: 0;
}
</style>
