<template>
  <div class="layout">
    <!-- grid overlay -->
    <div class="grid-bg" aria-hidden="true" />

    <!-- top bar -->
    <header class="topbar">
      <span class="topbar-tag">CI/CD</span>
      <span class="topbar-sep">|</span>
      <span class="topbar-env">ENV: <em>production</em></span>
      <span class="topbar-dot" :class="{ active: pipelineActive }" />
    </header>

    <!-- hero -->
    <main class="hero">
      <div class="eyebrow">
        <span class="bracket">[</span>
        <span class="eyebrow-text">PIPELINE STATUS</span>
        <span class="bracket">]</span>
      </div>

      <h1 class="heading">
        <span class="heading-line1">DevOps</span>
        <span class="heading-line2">CI/CD<span class="accent-dot">.</span>Demo</span>
      </h1>

      <p class="subtext">
        Automated build · test · deploy
      </p>

      <div class="action-row">
        <PipelineButton @click="pipelineActive = true" />
      </div>

      <!-- status strip -->
      <div class="status-strip">
        <div v-for="step in steps" :key="step.name" class="step" :class="step.state">
          <span class="step-dot" />
          <span class="step-name">{{ step.name }}</span>
        </div>
      </div>
    </main>

    <!-- footer -->
    <footer class="footer">
      <span>vue 3 + vite</span>
      <span class="sep">·</span>
      <span>production build</span>
    </footer>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
import PipelineButton from './components/PipelineButton.vue'

const pipelineActive = ref(false)

const steps = ref([
  { name: 'checkout', state: 'idle' },
  { name: 'install',  state: 'idle' },
  { name: 'test',     state: 'idle' },
  { name: 'build',    state: 'idle' },
  { name: 'deploy',   state: 'idle' },
])

watch(pipelineActive, (val) => {
  if (!val) return
  steps.value.forEach((step, i) => {
    setTimeout(() => { step.state = 'running' }, i * 320)
    setTimeout(() => { step.state = 'done'    }, i * 320 + 600)
  })
})
</script>

<style scoped>
/* ── Layout ── */
.layout {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  position: relative;
  overflow: hidden;
}

/* ── Grid background ── */
.grid-bg {
  position: fixed;
  inset: 0;
  pointer-events: none;
  background-image:
    linear-gradient(rgba(0,255,157,0.03) 1px, transparent 1px),
    linear-gradient(90deg, rgba(0,255,157,0.03) 1px, transparent 1px);
  background-size: 48px 48px;
  mask-image: radial-gradient(ellipse 80% 80% at 50% 50%, black 40%, transparent 100%);
}

/* ── Topbar ── */
.topbar {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 14px 32px;
  border-bottom: 1px solid var(--border);
  font-family: var(--font-mono);
  font-size: 0.72rem;
  color: var(--text-dim);
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.topbar-tag {
  color: var(--accent);
  font-weight: 700;
}

.topbar-sep { opacity: 0.3; }

.topbar em {
  font-style: normal;
  color: var(--accent2);
}

.topbar-dot {
  margin-left: auto;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--text-dim);
  transition: background 0.4s, box-shadow 0.4s;
}

.topbar-dot.active {
  background: var(--accent);
  box-shadow: 0 0 8px var(--accent);
  animation: pulse 2s ease infinite;
}

@keyframes pulse {
  0%, 100% { opacity: 1; }
  50%       { opacity: 0.5; }
}

/* ── Hero ── */
.hero {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 60px 24px;
  gap: 28px;
}

.eyebrow {
  font-family: var(--font-mono);
  font-size: 0.72rem;
  letter-spacing: 0.18em;
  color: var(--text-dim);
  display: flex;
  gap: 8px;
}

.bracket { color: var(--accent); opacity: 0.6; }
.eyebrow-text { color: var(--text-dim); }

/* heading */
.heading {
  display: flex;
  flex-direction: column;
  align-items: center;
  line-height: 1;
  gap: 4px;
}

.heading-line1 {
  font-family: var(--font-mono);
  font-size: clamp(2.8rem, 10vw, 5.5rem);
  font-weight: 700;
  color: var(--text);
  letter-spacing: -0.02em;
}

.heading-line2 {
  font-family: var(--font-mono);
  font-size: clamp(2.8rem, 10vw, 5.5rem);
  font-weight: 700;
  color: var(--accent);
  letter-spacing: -0.02em;
  text-shadow: 0 0 40px rgba(0,255,157,0.35);
}

.accent-dot { color: var(--accent2); }

/* subtext */
.subtext {
  font-size: 0.85rem;
  color: var(--text-dim);
  font-family: var(--font-mono);
  letter-spacing: 0.06em;
}

/* action */
.action-row {
  margin-top: 8px;
}

/* status strip */
.status-strip {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
  justify-content: center;
  margin-top: 8px;
}

.step {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 5px 12px;
  border: 1px solid var(--border);
  border-radius: 20px;
  font-family: var(--font-mono);
  font-size: 0.7rem;
  color: var(--text-dim);
  letter-spacing: 0.05em;
  transition: border-color 0.3s, color 0.3s;
}

.step.running {
  border-color: var(--accent2);
  color: var(--accent2);
}

.step.done {
  border-color: var(--accent);
  color: var(--accent);
}

.step-dot {
  width: 5px;
  height: 5px;
  border-radius: 50%;
  background: currentColor;
}

.step.running .step-dot {
  animation: pulse 0.6s ease infinite;
}

/* ── Footer ── */
.footer {
  padding: 16px 32px;
  border-top: 1px solid var(--border);
  font-family: var(--font-mono);
  font-size: 0.68rem;
  color: var(--text-dim);
  letter-spacing: 0.06em;
  text-transform: lowercase;
  display: flex;
  gap: 10px;
  justify-content: center;
}

.sep { opacity: 0.3; }
</style>
