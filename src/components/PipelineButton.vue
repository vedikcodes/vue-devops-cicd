<template>
  <button
    class="pipeline-btn"
    :class="{ triggered: fired, idle: !fired }"
    @click="trigger"
    :disabled="loading"
  >
    <span class="btn-icon">{{ loading ? '⟳' : fired ? '✓' : '▶' }}</span>
    <span class="btn-label">{{ label }}</span>
    <span class="btn-glow" />
  </button>
</template>

<script setup>
import { ref, computed } from 'vue'

const fired   = ref(false)
const loading = ref(false)

const label = computed(() => {
  if (loading.value) return 'Running...'
  if (fired.value)   return 'Pipeline Working!'
  return 'Run Pipeline'
})

function trigger() {
  if (fired.value || loading.value) return
  loading.value = true
  setTimeout(() => {
    loading.value = false
    fired.value   = true
  }, 1400)
}
</script>

<style scoped>
.pipeline-btn {
  position: relative;
  display: inline-flex;
  align-items: center;
  gap: 10px;
  padding: 14px 32px;
  border: 1.5px solid var(--border);
  border-radius: var(--radius);
  background: transparent;
  color: var(--text);
  font-family: var(--font-mono);
  font-size: 0.95rem;
  font-weight: 500;
  letter-spacing: 0.02em;
  cursor: pointer;
  overflow: hidden;
  transition: border-color 0.25s, color 0.25s, transform 0.15s;
  outline: none;
}

.pipeline-btn:hover:not(:disabled) {
  border-color: var(--accent);
  color: var(--accent);
  transform: translateY(-1px);
}

.pipeline-btn:active:not(:disabled) {
  transform: translateY(1px);
}

.pipeline-btn:disabled {
  cursor: default;
  opacity: 0.8;
}

/* triggered state */
.pipeline-btn.triggered {
  border-color: var(--accent);
  color: var(--accent);
  box-shadow: var(--glow);
}

.btn-icon {
  font-size: 1rem;
  transition: transform 0.3s;
}

.pipeline-btn.triggered .btn-icon {
  animation: pop 0.35s ease;
}

/* glow sweep on trigger */
.btn-glow {
  position: absolute;
  inset: 0;
  background: linear-gradient(90deg, transparent, rgba(0,255,157,0.08), transparent);
  transform: translateX(-100%);
  pointer-events: none;
}

.pipeline-btn.triggered .btn-glow {
  animation: sweep 0.6s ease forwards;
}

@keyframes sweep {
  to { transform: translateX(100%); }
}

@keyframes pop {
  0%   { transform: scale(1); }
  50%  { transform: scale(1.4); }
  100% { transform: scale(1); }
}

/* loading spin */
.pipeline-btn:disabled .btn-icon {
  display: inline-block;
  animation: spin 0.8s linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}
</style>
