<script setup lang="ts">
import { computed, ref, onUnmounted } from 'vue'

interface Player {
  name: string
  isAdmin?: boolean
}

const props = defineProps<{
  roomCode: string
  players: Player[]
  me?: string
}>()

const title = computed(() => `Room ${props.roomCode}`)

const isMe = (p: Player) => props.me && p.name.toLowerCase() === props.me.toLowerCase()

const emit = defineEmits<{
  (e: 'startGame'): void
  (e: 'leave'): void
}>()

const copied = ref(false)
let copiedTimer: number | undefined

async function copyRoomCode() {
  try {
    await navigator.clipboard.writeText(props.roomCode)
    copied.value = true
  } catch (_) {
    // Fallback for older browsers
    const el = document.createElement('textarea')
    el.value = props.roomCode
    el.setAttribute('readonly', '')
    el.style.position = 'absolute'
    el.style.left = '-9999px'
    document.body.appendChild(el)
    el.select()
    try { document.execCommand('copy') } catch (_) {}
    document.body.removeChild(el)
    copied.value = true
  }
  if (copiedTimer) window.clearTimeout(copiedTimer)
  copiedTimer = window.setTimeout(() => { copied.value = false }, 1400)
}

onUnmounted(() => { if (copiedTimer) window.clearTimeout(copiedTimer) })
</script>

<template>
  <section class="lobby" aria-label="Room lobby">
    <header class="lobby-header">
      <h2>{{ title }}</h2>
      <div class="code" aria-label="Room code">
        <span class="label">Code:</span>
        <span class="value">{{ roomCode }}</span>
        <button
          class="copy-btn"
          :class="{ copied: copied }"
          @click="copyRoomCode"
          :aria-label="`Copy room code ${roomCode}`"
          data-testid="btn-copy-code"
        >
          <span v-if="!copied">Copy</span>
          <span v-else>Copied!</span>
        </button>
      </div>
    </header>

    <div class="players">
      <h3>Players ({{ players.length }})</h3>
      <ul>
        <li v-for="p in players" :key="p.name" class="player">
          <span class="name">
            {{ p.name }}
            <small v-if="isMe(p)" class="me">(you)</small>
          </span>
          <span v-if="p.isAdmin" class="badge admin" aria-label="Admin">Admin</span>
        </li>
      </ul>
    </div>

    <footer class="actions">
      <button class="btn outline" @click="emit('leave')">Leave Room</button>
      <button class="btn primary" @click="emit('startGame')">Start Game</button>
    </footer>
  </section>
</template>

<style scoped>
.lobby {
  width: min(720px, 94vw);
  margin: 0 auto;
  background: rgba(15, 23, 42, 0.5);
  border: 1px solid rgba(148, 163, 184, 0.25);
  border-radius: 16px;
  padding: 24px;
  display: grid;
  gap: 16px;
}
.lobby-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
}
.code {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: #0b1220;
  border: 1px solid #334155;
  padding: 8px 12px;
  border-radius: 10px;
}
.code .label { color: #94a3b8; }
.code .value { font-weight: 700; letter-spacing: 0.08em; }
.code .copy-btn {
  margin-left: 8px;
  background: #0f172a;
  color: #e2e8f0;
  border: 1px solid #334155;
  padding: 6px 10px;
  border-radius: 8px;
  cursor: pointer;
  font-weight: 600;
}
.code .copy-btn:hover { border-color: #475569; }
.code .copy-btn.copied {
  background: #22c55e;
  color: #08230f;
  border-color: #16a34a;
}

.players h3 { margin: 0 0 8px; color: #cbd5e1; }
.players ul { list-style: none; padding: 0; margin: 0; display: grid; gap: 8px; }
.player {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: rgba(2, 6, 23, 0.6);
  border: 1px solid #1f2937;
  padding: 10px 12px;
  border-radius: 10px;
}
.name { color: #e2e8f0; font-weight: 600; }
.me { color: #94a3b8; font-weight: 400; margin-left: 6px; }
.badge.admin {
  background: #22c55e;
  color: #08230f;
  border: 1px solid #16a34a;
  border-radius: 999px;
  padding: 4px 10px;
  font-size: 0.85rem;
  font-weight: 700;
}

.actions { display: flex; justify-content: flex-end; gap: 10px; margin-top: 8px; }
.btn {
  padding: 10px 14px;
  border-radius: 10px;
  cursor: pointer;
  border: 1px solid transparent;
  transition: transform 0.05s ease-in-out;
}
.btn:active { transform: translateY(1px); }
.btn.primary { background: #22c55e; color: #08230f; border-color: #16a34a; font-weight: 800; }
.btn.outline { background: transparent; color: #e2e8f0; border-color: #334155; }
</style>
