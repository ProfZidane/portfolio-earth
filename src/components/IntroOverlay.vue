<template>
  <Transition name="overlay-out">
    <div v-if="visible" class="overlay" @click.self="dismiss">

      <div class="overlay-inner">

        <!-- Animated lines -->
        <div class="lines">
          <p
            v-for="(line, i) in lines"
            :key="i"
            class="line"
            :class="{ visible: step > i }"
            :style="`--i: ${i}`"
          >{{ line }}</p>
        </div>

        <!-- CTA -->
        <button
          class="cta-btn"
          :class="{ visible: step >= lines.length }"
          @click="dismiss"
        >
          <span>Start exploring</span>
          <svg class="cta-arrow" viewBox="0 0 20 20" fill="none">
            <path d="M4 10h12M11 5l5 5-5 5" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>

        <!-- Scroll hint -->
        <p class="hint" :class="{ visible: step >= lines.length }">
          or click anywhere to continue
        </p>

      </div>

      <!-- Corner decoration -->
      <div class="corner tl" aria-hidden="true"></div>
      <div class="corner tr" aria-hidden="true"></div>
      <div class="corner bl" aria-hidden="true"></div>
      <div class="corner br" aria-hidden="true"></div>

    </div>
  </Transition>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const emit = defineEmits(['done'])

const STORAGE_KEY = 'zm_intro_seen'

const lines = [
  'Explore my world.',
  'A journey through projects, ideas and experiences.',
  'Click on a country to discover what I\'ve built.',
]

const visible = ref(false)
const step = ref(0)

onMounted(() => {
  if (localStorage.getItem(STORAGE_KEY)) return

  visible.value = true

  // Stagger each line in
  lines.forEach((_, i) => {
    setTimeout(() => { step.value = i + 1 }, 600 + i * 900)
  })
})

const dismiss = () => {
  localStorage.setItem(STORAGE_KEY, '1')
  visible.value = false
  emit('done')
}
</script>

<style scoped>
/* ── Overlay ── */
.overlay {
  position: fixed;
  inset: 0;
  z-index: 90;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(3, 7, 16, 0.72);
  backdrop-filter: blur(6px);
  -webkit-backdrop-filter: blur(6px);
  cursor: pointer;
}

/* Fade-out when dismissed */
.overlay-out-leave-active {
  transition: opacity 0.8s ease, backdrop-filter 0.8s ease;
}
.overlay-out-leave-to {
  opacity: 0;
  backdrop-filter: blur(0px);
}

/* ── Inner content ── */
.overlay-inner {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
  text-align: center;
  padding: 0 32px;
  max-width: 640px;
  cursor: default;
  pointer-events: none; /* let clicks fall through to .overlay except on button */
}

/* ── Lines ── */
.lines {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.line {
  font-size: clamp(18px, 3vw, 26px);
  font-weight: 500;
  color: #fff;
  line-height: 1.4;
  opacity: 0;
  transform: translateY(14px);
  transition: opacity 0.7s ease, transform 0.7s ease;
  transition-delay: 0s;
}

/* First line gets a special treatment — larger */
.line:first-child {
  font-size: clamp(28px, 5vw, 48px);
  font-weight: 700;
  color: var(--cyan);
  text-shadow: 0 0 40px rgba(0, 255, 200, 0.4);
  letter-spacing: -0.5px;
}

.line.visible {
  opacity: 1;
  transform: translateY(0);
}

/* ── CTA button ── */
.cta-btn {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  margin-top: 12px;
  padding: 14px 32px;
  border-radius: 50px;
  border: 1px solid rgba(0, 255, 200, 0.4);
  background: rgba(0, 255, 200, 0.1);
  color: var(--cyan);
  font-family: 'Space Grotesk', sans-serif;
  font-size: 15px;
  font-weight: 600;
  letter-spacing: 0.3px;
  cursor: pointer;
  pointer-events: all;
  opacity: 0;
  transform: translateY(10px);
  transition: opacity 0.6s ease, transform 0.6s ease,
              background 0.25s, box-shadow 0.25s, border-color 0.25s;
}

.cta-btn.visible {
  opacity: 1;
  transform: translateY(0);
}

.cta-btn:hover {
  background: rgba(0, 255, 200, 0.2);
  border-color: rgba(0, 255, 200, 0.7);
  box-shadow: 0 0 32px rgba(0, 255, 200, 0.25);
}

.cta-btn:active {
  transform: scale(0.97);
}

.cta-arrow {
  width: 18px;
  height: 18px;
  transition: transform 0.25s ease;
}

.cta-btn:hover .cta-arrow {
  transform: translateX(4px);
}

/* ── Hint ── */
.hint {
  font-size: 12px;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  color: rgba(232, 234, 240, 0.25);
  opacity: 0;
  transition: opacity 0.6s ease 0.2s;
  pointer-events: none;
}

.hint.visible { opacity: 1; }

/* ── Corner decorations ── */
.corner {
  position: absolute;
  width: 24px;
  height: 24px;
  opacity: 0.3;
}

.corner.tl { top: 24px;    left: 24px;    border-top: 1px solid var(--cyan);  border-left: 1px solid var(--cyan); }
.corner.tr { top: 24px;    right: 24px;   border-top: 1px solid var(--cyan);  border-right: 1px solid var(--cyan); }
.corner.bl { bottom: 24px; left: 24px;    border-bottom: 1px solid var(--cyan); border-left: 1px solid var(--cyan); }
.corner.br { bottom: 24px; right: 24px;   border-bottom: 1px solid var(--cyan); border-right: 1px solid var(--cyan); }

/* ── Mobile ── */
@media (max-width: 768px) {
  .overlay-inner { padding: 0 24px; gap: 16px; }
  .cta-btn { padding: 12px 26px; font-size: 14px; }
}
</style>
