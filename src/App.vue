<script setup>
import { computed, ref } from 'vue'

const novaTarefa = ref('')
const tarefas = ref([])

const pendentes = computed(() => tarefas.value.filter(t => !t.concluida).length)

function adicionar() {
  if (novaTarefa.value.trim() !== '') {
    tarefas.value.push({ texto: novaTarefa.value, concluida: false })
    novaTarefa.value = ''
  }
}

function remover(index) {
  tarefas.value.splice(index, 1)
}
</script>

<template>
  <div class="page">
    <main class="board">
      <header class="board-header">
        <span class="eyebrow">Vue 3 · Composition API</span>
        <h1>Tarefas</h1>
      </header>

      <form class="input-row" @submit.prevent="adicionar">
        <input v-model="novaTarefa" placeholder="O que precisa ser feito?" aria-label="Nova tarefa" />
        <button class="add-btn" type="submit" aria-label="Adicionar tarefa">
          <svg viewBox="0 0 24 24" width="18" height="18" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><path d="M12 5v14M5 12h14"/></svg>
        </button>
      </form>

      <div class="status-row">
        <span v-if="tarefas.length" class="counter">{{ pendentes }} de {{ tarefas.length }} pendente(s)</span>
        <span v-else class="empty">Sem tarefas por aqui.</span>
      </div>

      <ul class="task-list">
        <li v-for="(t, index) in tarefas" :key="index" class="task-item" :class="{ done: t.concluida }">
          <label class="task-label">
            <input type="checkbox" v-model="t.concluida" aria-label="Marcar como concluída">
            <span class="check" aria-hidden="true">
              <svg viewBox="0 0 24 24" width="12" height="12" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><path d="M4 12l5 5L20 7"/></svg>
            </span>
            <span class="text">{{ t.texto }}</span>
          </label>
          <button class="remove-btn" @click="remover(index)" aria-label="Remover tarefa">
            <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round"><path d="M18 6L6 18M6 6l12 12"/></svg>
          </button>
        </li>
      </ul>
    </main>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Outfit:wght@400;500;600;700&display=swap');

* { box-sizing: border-box; }

.page {
  --bg: #101012;
  --surface: #18181b;
  --border: #2a2a2f;
  --text: #f2f2f0;
  --text-dim: #8b8b93;
  --accent: #d4ff4f;

  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--bg);
  font-family: 'Outfit', 'Segoe UI', Arial, sans-serif;
  padding: 24px;
}

.board {
  width: 100%;
  max-width: 440px;
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 14px;
  padding: 28px;
}

.board-header {
  margin-bottom: 22px;
}

.eyebrow {
  display: block;
  font-size: 0.72rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--accent);
  margin-bottom: 6px;
}

h1 {
  margin: 0;
  color: var(--text);
  font-size: 1.7rem;
  font-weight: 700;
  letter-spacing: -0.01em;
}

.input-row {
  display: flex;
  gap: 8px;
  margin-bottom: 18px;
}

.input-row input {
  flex: 1;
  min-width: 0;
  min-height: 44px;
  padding: 0 14px;
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: 9px;
  color: var(--text);
  font: inherit;
  font-size: 0.95rem;
  outline: none;
  transition: border-color 150ms ease;
}

.input-row input::placeholder {
  color: var(--text-dim);
}

.input-row input:focus {
  border-color: var(--accent);
}

.add-btn {
  width: 44px;
  height: 44px;
  flex-shrink: 0;
  display: grid;
  place-items: center;
  background: var(--accent);
  color: #101012;
  border: none;
  border-radius: 9px;
  cursor: pointer;
  transition: transform 150ms ease, background 150ms ease;
}

.add-btn:hover {
  background: #c2ec2e;
}

.add-btn:active {
  transform: scale(0.94);
}

.status-row {
  margin-bottom: 14px;
  font-size: 0.82rem;
}

.counter {
  color: var(--text-dim);
}

.empty {
  color: var(--text-dim);
}

.task-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.task-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 10px;
  padding: 12px 2px;
  border-bottom: 1px solid var(--border);
}

.task-item:last-child {
  border-bottom: none;
}

.task-label {
  display: flex;
  align-items: center;
  gap: 12px;
  cursor: pointer;
  overflow: hidden;
  min-height: 44px;
  flex: 1;
}

.task-label input[type="checkbox"] {
  position: absolute;
  width: 1px;
  height: 1px;
  opacity: 0;
}

.check {
  flex-shrink: 0;
  width: 20px;
  height: 20px;
  border: 1.5px solid var(--border);
  border-radius: 6px;
  display: grid;
  place-items: center;
  color: transparent;
  transition: background 150ms ease, border-color 150ms ease, color 150ms ease;
}

.task-item.done .check {
  background: var(--accent);
  border-color: var(--accent);
  color: #101012;
}

.text {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  color: var(--text);
  font-size: 0.95rem;
}

.task-item.done .text {
  text-decoration: line-through;
  color: var(--text-dim);
}

.remove-btn {
  width: 32px;
  height: 32px;
  flex-shrink: 0;
  display: grid;
  place-items: center;
  background: none;
  border: none;
  color: var(--text-dim);
  cursor: pointer;
  border-radius: 7px;
  transition: background 150ms ease, color 150ms ease;
}

.remove-btn:hover {
  background: #2a2a2f;
  color: #ff6b6b;
}
</style>