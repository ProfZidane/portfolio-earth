<template>
  <div class="about-page">
    <div class="about-inner">

      <!-- Who I Am -->
      <div class="about-block fade-in" style="--delay: 0s">
        <p class="label">Who I Am</p>
        <h1 class="hero-title">Builder of systems<br><span class="accent">across the world.</span></h1>
        <p class="hero-sub">
          AI Engineer, Product Builder &amp; Tech Lead — I design and ship intelligent systems 
          that solve real problems across diverse and multicultural environments.
        </p>
      </div>

      <div class="divider"></div>

      <!-- My Vision -->
      <div class="about-block fade-in" style="--delay: 0.1s">
        <p class="label">My Vision</p>
        <p class="body-text">
          I believe the intersection of <span class="hl">AI, product thinking, and engineering</span> is where
          the most impactful systems are built. My goal is to create tools that empower
          businesses and people — not just technically, but meaningfully.
        </p>
      </div>

      <div class="divider"></div>

      <!-- Stats Dashboard (replaces My Journey) -->
      <div class="about-block fade-in" style="--delay: 0.2s">
        <p class="label">By the Numbers</p>

        <!-- Signal pulse decoration -->
        <div class="signal-bar" aria-hidden="true">
          <span v-for="i in 12" :key="i" class="signal-tick" :style="`--i:${i}`"></span>
        </div>

        <div class="stats-grid">
          <div
            v-for="stat in stats"
            :key="stat.label"
            class="stat-card"
          >
            <div class="stat-icon">{{ stat.icon }}</div>
            <div class="stat-value">
              <span class="counter" :data-target="stat.raw">{{ displayValues[stat.label] }}</span>
              <span v-if="stat.suffix" class="stat-suffix">{{ stat.suffix }}</span>
            </div>
            <p class="stat-label">{{ stat.label }}</p>
          </div>
        </div>
      </div>

      <div class="divider"></div>

      <!-- Skills -->
      <div class="about-block fade-in" style="--delay: 0.3s">
        <p class="label">Skills</p>
        <div class="skills-grid">
          <div v-for="group in skills" :key="group.category" class="skill-group">
            <p class="skill-category">{{ group.category }}</p>
            <div class="skill-tags">
              <span v-for="s in group.items" :key="s" class="skill-tag">{{ s }}</span>
            </div>
          </div>
        </div>
      </div>

      <div class="divider"></div>

      <!-- Connect -->
      <div class="about-block fade-in" style="--delay: 0.4s">
        <p class="label">Connect</p>
        <div class="connect-grid">
          <a href="mailto:your@email.com" class="connect-btn primary">
            <span class="btn-icon">✉</span> Contact Me
          </a>
          <a href="https://linkedin.com/in/yourprofile" target="_blank" rel="noopener" class="connect-btn">
            <span class="btn-icon">in</span> LinkedIn
          </a>
          <a href="https://medium.com/@yourprofile" target="_blank" rel="noopener" class="connect-btn">
            <span class="btn-icon">✍</span> Articles
          </a>
        </div>
      </div>

    </div>
  </div>
</template>

<script setup>
import { reactive, onMounted } from 'vue'
import { countriesData } from '../data/experiences.js'
import { products } from '../data/products.js'

// Derive total unique companies across all countries
const totalCompanies = countriesData.reduce((acc, c) => {
  c.experiences.forEach(e => {
    if (!acc.includes(e.company)) acc.push(e.company)
  })
  return acc
}, []).length

const stats = [
  { label: 'Projects Built',      icon: '⬡', raw: products.length,        suffix: null },
  { label: 'Countries Worked',    icon: '◎', raw: countriesData.length,   suffix: null },
  { label: 'Companies & Clients', icon: '◈', raw: totalCompanies,         suffix: '+' },
  { label: 'Years of Experience', icon: '◷', raw: 5,                      suffix: '+' },
]

// Animated counter state
const displayValues = reactive(
  Object.fromEntries(stats.map(s => [s.label, 0]))
)

onMounted(() => {
  stats.forEach(stat => {
    const duration = 1400
    const steps = 40
    const increment = stat.raw / steps
    let current = 0
    let step = 0
    // stagger each counter slightly
    const delay = stats.indexOf(stat) * 120

    setTimeout(() => {
      const timer = setInterval(() => {
        step++
        current = Math.min(Math.round(increment * step), stat.raw)
        displayValues[stat.label] = current
        if (current >= stat.raw) clearInterval(timer)
      }, duration / steps)
    }, delay)
  })
})

const skills = [
  { category: 'AI & Data',    items: ['LLMs', 'RAG', 'MLOps', 'ETL', 'Python', 'TensorFlow'] },
  { category: 'Engineering',  items: ['Vue.js', 'Angular', 'Spring Boot', 'Node.js', 'Docker'] },
  { category: 'Product',      items: ['System Design', 'SaaS Architecture', 'CTO', 'Agile'] },
]
</script>

<style scoped>
.about-page {
  position: fixed;
  inset: 0;
  background: #050a12;
  overflow-y: auto;
  padding: 100px 0 80px;
}

.about-inner {
  max-width: 760px;
  margin: 0 auto;
  padding: 0 40px;
  display: flex;
  flex-direction: column;
}

.about-block {
  padding: 40px 0;
  animation: fadeUp 0.6s ease both;
  animation-delay: var(--delay, 0s);
}

@keyframes fadeUp {
  from { opacity: 0; transform: translateY(24px); }
  to   { opacity: 1; transform: translateY(0); }
}

.divider { height: 1px; background: var(--glass-border); }

.label {
  font-size: 11px;
  letter-spacing: 3px;
  text-transform: uppercase;
  color: var(--cyan);
  margin-bottom: 16px;
}

/* ── Hero ── */
.hero-title {
  font-size: 52px;
  font-weight: 700;
  line-height: 1.1;
  color: #fff;
  margin-bottom: 20px;
}
.accent { color: var(--cyan); }
.hero-sub {
  font-size: 17px;
  line-height: 1.7;
  color: var(--text-muted);
  max-width: 580px;
}

/* ── Vision ── */
.body-text { font-size: 16px; line-height: 1.8; color: rgba(232, 234, 240, 0.75); }
.hl { color: var(--cyan); font-weight: 500; }

/* ── Signal bar ── */
.signal-bar {
  display: flex;
  align-items: flex-end;
  gap: 3px;
  height: 28px;
  margin-bottom: 28px;
}

.signal-tick {
  display: block;
  width: 3px;
  border-radius: 2px;
  background: var(--cyan);
  opacity: 0.15;
  animation: pulse-tick 1.6s ease-in-out infinite;
  animation-delay: calc(var(--i) * 0.1s);
  /* vary heights to look like a signal waveform */
  height: calc(20% + (var(--i) * 6%));
}

@keyframes pulse-tick {
  0%, 100% { opacity: 0.12; transform: scaleY(0.6); }
  50%       { opacity: 0.7;  transform: scaleY(1); }
}

/* ── Stats grid ── */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px;
}

.stat-card {
  background: var(--glass-bg);
  border: 1px solid var(--glass-border);
  border-radius: 18px;
  padding: 24px 16px 20px;
  text-align: center;
  cursor: default;
  transition: transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1),
              border-color 0.3s ease,
              box-shadow 0.3s ease;
  position: relative;
  overflow: hidden;
}

.stat-card::before {
  content: '';
  position: absolute;
  inset: 0;
  background: radial-gradient(circle at 50% 0%, rgba(0, 255, 200, 0.06) 0%, transparent 70%);
  opacity: 0;
  transition: opacity 0.3s;
}

.stat-card:hover {
  transform: translateY(-6px) scale(1.03);
  border-color: var(--cyan-glow);
  box-shadow: 0 0 30px rgba(0, 255, 200, 0.12), 0 16px 40px rgba(0, 0, 0, 0.5);
}

.stat-card:hover::before { opacity: 1; }

.stat-icon {
  font-size: 20px;
  color: var(--cyan);
  opacity: 0.5;
  margin-bottom: 12px;
  line-height: 1;
}

.stat-value {
  display: flex;
  align-items: baseline;
  justify-content: center;
  gap: 2px;
  margin-bottom: 8px;
}

.counter {
  font-size: 42px;
  font-weight: 700;
  color: #fff;
  line-height: 1;
  font-variant-numeric: tabular-nums;
}

.stat-suffix {
  font-size: 22px;
  font-weight: 600;
  color: var(--cyan);
  line-height: 1;
}

.stat-label {
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: var(--text-muted);
  line-height: 1.4;
}

/* ── Skills ── */
.skills-grid { display: flex; flex-direction: column; gap: 20px; }

.skill-category {
  font-size: 13px;
  font-weight: 600;
  color: var(--text-muted);
  margin-bottom: 10px;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.skill-tags { display: flex; flex-wrap: wrap; gap: 8px; }

.skill-tag {
  font-size: 13px;
  font-weight: 500;
  color: #e8eaf0;
  background: var(--surface);
  border: 1px solid var(--glass-border);
  padding: 5px 14px;
  border-radius: 20px;
  transition: 0.2s;
}
.skill-tag:hover {
  border-color: var(--cyan-glow);
  color: var(--cyan);
  background: var(--cyan-dim);
}

/* ── Connect ── */
.connect-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}

.connect-btn {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  padding: 12px 24px;
  border-radius: 50px;
  font-size: 14px;
  font-weight: 600;
  font-family: 'Space Grotesk', sans-serif;
  text-decoration: none;
  border: 1px solid var(--glass-border);
  background: var(--surface);
  color: rgba(232, 234, 240, 0.8);
  transition: transform 0.25s ease, border-color 0.25s, box-shadow 0.25s, color 0.25s, background 0.25s;
  cursor: pointer;
}

.connect-btn:hover {
  transform: translateY(-3px);
  border-color: var(--cyan-glow);
  box-shadow: 0 0 24px rgba(0, 255, 200, 0.15);
  color: var(--cyan);
  background: var(--cyan-dim);
}

.connect-btn.primary {
  background: var(--cyan-dim);
  border-color: rgba(0, 255, 200, 0.35);
  color: var(--cyan);
}

.connect-btn.primary:hover {
  background: rgba(0, 255, 200, 0.22);
  box-shadow: 0 0 32px rgba(0, 255, 200, 0.25);
}

.btn-icon {
  font-size: 15px;
  line-height: 1;
}

/* ── Responsive ── */
@media (max-width: 768px) {
  .about-inner { padding: 0 20px; }
  .hero-title { font-size: 36px; }
  .stats-grid { grid-template-columns: repeat(2, 1fr); }
  .counter { font-size: 34px; }
}
</style>
