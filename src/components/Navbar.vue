<template>
  <nav class="navbar">
    <div class="logo">ZM</div>
    <div class="nav-links">
      <button
        v-for="item in items"
        :key="item.id"
        :class="['nav-btn', { active: current === item.id }]"
        @click="$emit('navigate', item.id)"
      >
        {{ item.label }}
      </button>
    </div>
  </nav>
</template>

<script setup>
defineProps({ current: String })
defineEmits(['navigate'])

const items = [
  { id: 'world', label: 'World' },
  { id: 'products', label: 'Product Islands' },
  { id: 'about', label: 'About' }
]
</script>

<style scoped>
.navbar {
  position: fixed;
  top: 20px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 100;
  display: flex;
  align-items: center;
  gap: 32px;
  padding: 10px 24px;
  background: var(--glass-bg);
  border: 1px solid var(--glass-border);
  border-radius: 50px;
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  box-shadow: 0 0 40px rgba(0, 255, 200, 0.06);
}

.logo {
  font-weight: 700;
  font-size: 16px;
  color: var(--cyan);
  letter-spacing: 2px;
}

.nav-links {
  display: flex;
  gap: 4px;
}

.nav-btn {
  background: none;
  border: none;
  color: var(--text-muted);
  font-family: 'Space Grotesk', sans-serif;
  font-size: 14px;
  font-weight: 500;
  padding: 6px 16px;
  border-radius: 30px;
  cursor: pointer;
  transition: color 0.25s, background 0.25s;
}

.nav-btn:hover {
  color: #e8eaf0;
  background: var(--surface);
}

.nav-btn.active {
  color: var(--cyan);
  background: var(--cyan-dim);
}
</style>
