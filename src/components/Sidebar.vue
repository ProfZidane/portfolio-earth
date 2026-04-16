<template>
  <Transition name="sidebar">
    <aside v-if="country" class="sidebar">
      <div class="sidebar-header">
        <div>
          <p class="sidebar-label">Explored Country</p>
          <h2 class="sidebar-title">{{ country.name }}</h2>
        </div>
        <button class="close-btn" @click="$emit('close')">✕</button>
      </div>

      <div class="sidebar-body">

        <!-- Experiences -->
        <section v-if="country.experiences?.length">
          <h3 class="section-title">
            <span class="dot"></span> Experiences
          </h3>
          <div
            v-for="exp in country.experiences"
            :key="exp.company + exp.period"
            class="exp-card"
          >
            <div class="exp-top">
              <div>
                <p class="company">{{ exp.company }}</p>
                <p class="role">{{ exp.role }}</p>
              </div>
              <span class="period">{{ exp.period }}</span>
            </div>
            <p class="context">{{ exp.story.context }}</p>
            <p class="challenge"><strong>Challenge:</strong> {{ exp.story.challenge }}</p>
            <ul class="actions">
              <li v-for="a in exp.story.actions" :key="a">{{ a }}</li>
            </ul>
            <p class="impact">{{ exp.story.impact }}</p>
          </div>
        </section>

        <!-- Awards -->
        <section v-if="country.awards?.length">
          <h3 class="section-title">
            <span class="dot award-dot"></span> Awards
          </h3>
          <div v-for="award in country.awards" :key="award.title" class="award-card">
            <p class="award-title">🏆 {{ award.title }}</p>
            <p class="award-desc">{{ award.description }}</p>
          </div>
        </section>

      </div>
    </aside>
  </Transition>
</template>

<script setup>
defineProps({ country: Object })
defineEmits(['close'])
</script>

<style scoped>
.sidebar {
  position: fixed;
  top: 0;
  right: 0;
  width: 38%;
  min-width: 340px;
  max-width: 560px;
  height: 100vh;
  z-index: 50;
  background: var(--glass-bg);
  border-left: 1px solid var(--glass-border);
  backdrop-filter: blur(28px);
  -webkit-backdrop-filter: blur(28px);
  display: flex;
  flex-direction: column;
  box-shadow: -20px 0 80px rgba(0, 0, 0, 0.6);
}

/* Slide transition */
.sidebar-enter-active, .sidebar-leave-active {
  transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1), opacity 0.4s ease;
}
.sidebar-enter-from, .sidebar-leave-to {
  transform: translateX(100%);
  opacity: 0;
}

.sidebar-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  padding: 28px 24px 20px;
  border-bottom: 1px solid var(--glass-border);
  flex-shrink: 0;
}

.sidebar-label {
  font-size: 11px;
  letter-spacing: 2px;
  text-transform: uppercase;
  color: var(--cyan);
  margin-bottom: 4px;
}

.sidebar-title {
  font-size: 26px;
  font-weight: 700;
  color: #fff;
}

.close-btn {
  background: var(--surface);
  border: 1px solid var(--glass-border);
  color: var(--text-muted);
  width: 32px;
  height: 32px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 13px;
  transition: 0.25s;
  flex-shrink: 0;
}
.close-btn:hover {
  background: var(--cyan-dim);
  color: var(--cyan);
  border-color: var(--cyan-glow);
}

.sidebar-body {
  flex: 1;
  overflow-y: auto;
  padding: 24px;
  display: flex;
  flex-direction: column;
  gap: 32px;
}

.section-title {
  font-size: 12px;
  font-weight: 600;
  letter-spacing: 2px;
  text-transform: uppercase;
  color: var(--text-muted);
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 14px;
}

.dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: var(--cyan);
  box-shadow: 0 0 8px var(--cyan);
  flex-shrink: 0;
}

.award-dot { background: #ffaa00; box-shadow: 0 0 8px #ffaa00; }

.exp-card {
  background: var(--surface);
  border: 1px solid var(--glass-border);
  border-radius: 14px;
  padding: 16px;
  margin-bottom: 12px;
  transition: border-color 0.25s, box-shadow 0.25s;
}
.exp-card:hover {
  border-color: var(--cyan-glow);
  box-shadow: 0 0 24px rgba(0, 255, 200, 0.1);
}

.exp-top {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 10px;
}

.company {
  font-weight: 600;
  font-size: 15px;
  color: var(--cyan);
}

.role {
  font-size: 12px;
  color: var(--text-muted);
  margin-top: 2px;
}

.period {
  font-size: 11px;
  color: var(--text-muted);
  white-space: nowrap;
  margin-left: 8px;
}

.context {
  font-size: 13px;
  line-height: 1.6;
  color: rgba(232, 234, 240, 0.75);
  margin-bottom: 8px;
}

.challenge {
  font-size: 13px;
  color: #ffaa00;
  margin-bottom: 8px;
}
.challenge strong { font-weight: 600; }

.actions {
  list-style: none;
  margin-bottom: 10px;
  display: flex;
  flex-direction: column;
  gap: 4px;
}
.actions li {
  font-size: 13px;
  color: rgba(232, 234, 240, 0.7);
  padding-left: 14px;
  position: relative;
}
.actions li::before {
  content: '';
  position: absolute;
  left: 0;
  top: 7px;
  width: 4px;
  height: 4px;
  border-radius: 50%;
  background: var(--cyan);
}

.impact {
  font-size: 13px;
  font-weight: 500;
  color: var(--cyan);
  padding-top: 8px;
  border-top: 1px solid var(--glass-border);
}

.award-card {
  background: var(--surface);
  border: 1px solid rgba(255, 170, 0, 0.15);
  border-radius: 12px;
  padding: 14px 16px;
  margin-bottom: 10px;
  transition: 0.25s;
}
.award-card:hover {
  border-color: rgba(255, 170, 0, 0.4);
  box-shadow: 0 0 20px rgba(255, 170, 0, 0.1);
}

.award-title {
  font-size: 14px;
  font-weight: 600;
  color: #ffaa00;
  margin-bottom: 4px;
}

.award-desc {
  font-size: 12px;
  color: var(--text-muted);
}
</style>
