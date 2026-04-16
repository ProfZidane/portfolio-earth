<template>
  <div class="about-page">
    <div class="about-inner">

      <div class="about-block fade-in" style="--delay: 0s">
        <p class="label">Who I Am</p>
        <h1 class="hero-title">Builder of systems<br><span class="accent">across the world.</span></h1>
        <p class="hero-sub">
          AI Engineer, Product Builder & CTO — I design and ship intelligent systems
          that solve real problems, from West Africa to Europe and North America.
        </p>
      </div>

      <div class="divider"></div>

      <div class="about-block fade-in" style="--delay: 0.1s">
        <p class="label">My Vision</p>
        <p class="body-text">
          I believe the intersection of <span class="hl">AI, product thinking, and engineering</span> is where
          the most impactful systems are built. My goal is to create tools that empower
          businesses and people — not just technically, but meaningfully.
        </p>
      </div>

      <div class="divider"></div>

      <div class="about-block fade-in" style="--delay: 0.2s">
        <p class="label">My Journey</p>
        <div class="journey-grid">
          <div v-for="stop in journey" :key="stop.country" class="journey-card">
            <p class="journey-flag">{{ stop.flag }}</p>
            <p class="journey-country">{{ stop.country }}</p>
            <p class="journey-desc">{{ stop.desc }}</p>
          </div>
        </div>
      </div>

      <div class="divider"></div>

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

    </div>
  </div>
</template>

<script setup>
const journey = [
  { flag: '🇨🇮', country: 'Ivory Coast', desc: 'Founded DigitLab & Z-SUITE. Built the first products.' },
  { flag: '🇨🇦', country: 'Canada', desc: 'Freelance CTO at Intevoo. Scaled SaaS products.' },
  { flag: '🇹🇳', country: 'Tunisia', desc: 'Data engineering & AI systems at Upgradetek.' },
  { flag: '🇫🇷', country: 'France', desc: 'AI Engineer at La Forge. Production-grade AI.' },
]

const skills = [
  { category: 'AI & Data', items: ['LLMs', 'RAG', 'MLOps', 'ETL', 'Python', 'TensorFlow'] },
  { category: 'Engineering', items: ['Vue.js', 'Angular', 'Spring Boot', 'Node.js', 'Docker'] },
  { category: 'Product', items: ['System Design', 'SaaS Architecture', 'CTO', 'Agile'] },
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
  gap: 0;
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

.divider {
  height: 1px;
  background: var(--glass-border);
}

.label {
  font-size: 11px;
  letter-spacing: 3px;
  text-transform: uppercase;
  color: var(--cyan);
  margin-bottom: 16px;
}

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

.body-text {
  font-size: 16px;
  line-height: 1.8;
  color: rgba(232, 234, 240, 0.75);
}

.hl { color: var(--cyan); font-weight: 500; }

.journey-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 16px;
}

.journey-card {
  background: var(--surface);
  border: 1px solid var(--glass-border);
  border-radius: 14px;
  padding: 18px;
  transition: border-color 0.25s, box-shadow 0.25s;
}

.journey-card:hover {
  border-color: var(--cyan-glow);
  box-shadow: 0 0 20px rgba(0, 255, 200, 0.08);
}

.journey-flag { font-size: 28px; margin-bottom: 6px; }
.journey-country { font-size: 15px; font-weight: 600; color: #fff; margin-bottom: 6px; }
.journey-desc { font-size: 13px; color: var(--text-muted); line-height: 1.5; }

.skills-grid {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.skill-category {
  font-size: 13px;
  font-weight: 600;
  color: var(--text-muted);
  margin-bottom: 10px;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.skill-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

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
</style>
