<template>
  <Teleport to="body">
    <Transition name="modal">
      <div v-if="product" class="modal-backdrop" @click.self="$emit('close')">
        <div class="modal-box">

          <!-- Close -->
          <button class="modal-close" @click="$emit('close')">✕</button>

          <!-- Left: image carousel -->
          <div class="modal-media">
            <div class="carousel-track">
              <Transition :name="slideDir" mode="out-in">
                <img
                  :key="imgIndex"
                  :src="product.images[imgIndex]"
                  :alt="product.title"
                  class="carousel-img"
                />
              </Transition>
            </div>

            <!-- Arrows -->
            <button
              v-if="product.images.length > 1"
              class="arrow arrow-left"
              @click="prev"
            >‹</button>
            <button
              v-if="product.images.length > 1"
              class="arrow arrow-right"
              @click="next"
            >›</button>

            <!-- Dots -->
            <div class="carousel-dots" v-if="product.images.length > 1">
              <span
                v-for="(_, i) in product.images"
                :key="i"
                :class="['cdot', { active: imgIndex === i }]"
                @click="goTo(i)"
              />
            </div>
          </div>

          <!-- Right: content -->
          <div class="modal-content">
            <div class="modal-tags">
              <span v-for="tag in product.tags" :key="tag" class="tag">{{ tag }}</span>
            </div>

            <h2 class="modal-title">{{ product.title }}</h2>
            <p class="modal-tagline">{{ product.tagline }}</p>
            <p class="modal-desc">{{ product.description }}</p>

            <div class="modal-section">
              <p class="section-label">Key Features</p>
              <ul class="features-list">
                <li v-for="f in product.features" :key="f">{{ f }}</li>
              </ul>
            </div>

            <div class="modal-section">
              <p class="section-label">Tech Stack</p>
              <div class="tech-tags">
                <span v-for="t in product.tech" :key="t" class="tech-tag">{{ t }}</span>
              </div>
            </div>
          </div>

        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup>
import { ref, watch } from 'vue'

const props = defineProps({ product: Object })
const emit = defineEmits(['close'])

const imgIndex = ref(0)
const slideDir = ref('slide-next')

const next = () => {
  slideDir.value = 'slide-next'
  imgIndex.value = (imgIndex.value + 1) % props.product.images.length
}
const prev = () => {
  slideDir.value = 'slide-prev'
  imgIndex.value = (imgIndex.value - 1 + props.product.images.length) % props.product.images.length
}
const goTo = (i) => {
  slideDir.value = i > imgIndex.value ? 'slide-next' : 'slide-prev'
  imgIndex.value = i
}

const onKey = (e) => { if (e.key === 'Escape') emit('close') }

watch(() => props.product, (val) => {
  imgIndex.value = 0
  if (val) window.addEventListener('keydown', onKey)
  else window.removeEventListener('keydown', onKey)
}, { immediate: true })
</script>

<style scoped>
/* ── Backdrop ── */
.modal-backdrop {
  position: fixed;
  inset: 0;
  z-index: 200;
  background: rgba(2, 6, 14, 0.88);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 24px;
}

/* ── Box ── */
.modal-box {
  position: relative;
  display: grid;
  grid-template-columns: 1fr 1fr;
  width: 100%;
  max-width: 1080px;
  max-height: 88vh;
  background: rgba(8, 14, 26, 0.96);
  border: 1px solid rgba(0, 255, 200, 0.15);
  border-radius: 24px;
  overflow: hidden;
  box-shadow: 0 0 80px rgba(0, 255, 200, 0.08), 0 40px 120px rgba(0, 0, 0, 0.8);
}

/* ── Modal transition (zoom) ── */
.modal-enter-active { transition: opacity 0.35s ease, transform 0.35s cubic-bezier(0.34, 1.56, 0.64, 1); }
.modal-leave-active { transition: opacity 0.25s ease, transform 0.25s ease; }
.modal-enter-from  { opacity: 0; transform: scale(0.88); }
.modal-leave-to    { opacity: 0; transform: scale(0.94); }

/* ── Close ── */
.modal-close {
  position: absolute;
  top: 16px;
  right: 16px;
  z-index: 10;
  width: 34px;
  height: 34px;
  border-radius: 50%;
  border: 1px solid rgba(0, 255, 200, 0.2);
  background: rgba(8, 14, 26, 0.8);
  color: rgba(232, 234, 240, 0.6);
  font-size: 14px;
  cursor: pointer;
  transition: 0.2s;
}
.modal-close:hover {
  background: rgba(0, 255, 200, 0.15);
  color: var(--cyan);
  border-color: var(--cyan-glow);
}

/* ── Media (left) ── */
.modal-media {
  position: relative;
  background: #020810;
  overflow: hidden;
  min-height: 400px;
}

.carousel-track {
  width: 100%;
  height: 100%;
}

.carousel-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

/* Carousel slide transitions */
.slide-next-enter-active,
.slide-next-leave-active,
.slide-prev-enter-active,
.slide-prev-leave-active {
  transition: opacity 0.3s ease, transform 0.3s ease;
  position: absolute;
  inset: 0;
}
.slide-next-enter-from  { opacity: 0; transform: translateX(40px); }
.slide-next-leave-to    { opacity: 0; transform: translateX(-40px); }
.slide-prev-enter-from  { opacity: 0; transform: translateX(-40px); }
.slide-prev-leave-to    { opacity: 0; transform: translateX(40px); }

/* Arrows */
.arrow {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  z-index: 5;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  border: 1px solid rgba(0, 255, 200, 0.2);
  background: rgba(8, 14, 26, 0.7);
  color: #fff;
  font-size: 22px;
  line-height: 1;
  cursor: pointer;
  transition: 0.2s;
  display: flex;
  align-items: center;
  justify-content: center;
}
.arrow:hover { background: rgba(0, 255, 200, 0.2); border-color: var(--cyan); color: var(--cyan); }
.arrow-left  { left: 12px; }
.arrow-right { right: 12px; }

/* Dots */
.carousel-dots {
  position: absolute;
  bottom: 14px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 7px;
  z-index: 5;
}
.cdot {
  width: 7px;
  height: 7px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.25);
  cursor: pointer;
  transition: 0.2s;
}
.cdot.active { background: var(--cyan); box-shadow: 0 0 8px var(--cyan); }

/* ── Content (right) ── */
.modal-content {
  padding: 40px 36px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.modal-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
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

.modal-title {
  font-size: 32px;
  font-weight: 700;
  color: #fff;
  line-height: 1.1;
}

.modal-tagline {
  font-size: 15px;
  font-weight: 500;
  color: var(--cyan);
  line-height: 1.5;
}

.modal-desc {
  font-size: 14px;
  line-height: 1.8;
  color: rgba(232, 234, 240, 0.7);
}

.modal-section { display: flex; flex-direction: column; gap: 10px; }

.section-label {
  font-size: 10px;
  font-weight: 600;
  letter-spacing: 2px;
  text-transform: uppercase;
  color: rgba(232, 234, 240, 0.4);
}

.features-list {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 7px;
}
.features-list li {
  font-size: 13px;
  color: rgba(232, 234, 240, 0.75);
  padding-left: 16px;
  position: relative;
  line-height: 1.5;
}
.features-list li::before {
  content: '';
  position: absolute;
  left: 0;
  top: 7px;
  width: 5px;
  height: 5px;
  border-radius: 50%;
  background: var(--cyan);
  box-shadow: 0 0 6px var(--cyan);
}

.tech-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 7px;
}
.tech-tag {
  font-size: 12px;
  font-weight: 500;
  color: rgba(232, 234, 240, 0.7);
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(255, 255, 255, 0.08);
  padding: 4px 12px;
  border-radius: 6px;
  transition: 0.2s;
}
.tech-tag:hover {
  border-color: rgba(0, 255, 200, 0.3);
  color: var(--cyan);
}
</style>
