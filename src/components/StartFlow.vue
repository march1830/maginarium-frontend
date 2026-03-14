<script setup lang="ts">
import { ref, computed } from 'vue'

// Emits for parent integration later
const emit = defineEmits<{
  (e: 'create', payload: { name: string }): void
  (e: 'join', payload: { name: string; roomCode: string }): void
}>()

type Mode = 'menu' | 'create' | 'join'
const mode = ref<Mode>('menu')

// Use a single dynamic transition name to avoid UI twitching between views
const transitionName = computed(() => {
  if (mode.value === 'menu') return 'fade'
  return mode.value === 'create' ? 'slide-down' : 'slide-up'
})

const name = ref('')
const roomCode = ref('')

const canCreate = computed(() => name.value.trim().length > 0)
const canJoin = computed(() => canCreate.value && /^[A-Za-z0-9\-]{3,12}$/.test(roomCode.value.trim()))

function goCreate() {
  mode.value = 'create'
}
function goJoin() {
  mode.value = 'join'
}
function goBack() {
  mode.value = 'menu'
}

function onStart() {
  if (!canCreate.value) return
  emit('create', { name: name.value.trim() })
}

function onJoin() {
  if (!canJoin.value) return
  emit('join', { name: name.value.trim(), roomCode: roomCode.value.trim().toUpperCase() })
}
</script>

<template>
  <div class="start-flow">
    <!-- Single transition wrapper to avoid layout thrash between views -->
    <Transition :name="transitionName" mode="out-in">
      <!-- Menu -->
      <section v-if="mode === 'menu'" key="menu" class="panel menu" aria-label="Start menu">
        <button class="cta primary" @click="goCreate" data-testid="btn-new">New Game</button>
        <div class="or">or</div>
        <button class="cta outline" @click="goJoin" data-testid="btn-join">Join</button>
      </section>

      <!-- Create (slides from top area expanding downward) -->
      <section v-else-if="mode === 'create'" key="create" class="panel form create" aria-label="Create game">
        <button class="back" @click="goBack" aria-label="Back to menu">Back</button>
        <label for="name-create">Name</label>
        <input
          id="name-create"
          v-model="name"
          type="text"
          placeholder="Your name"
          autocomplete="name"
          @keyup.enter="onStart"
        />
        <button class="cta primary" :disabled="!canCreate" @click="onStart">Start</button>
      </section>

      <!-- Join (slides up from bottom) -->
      <section v-else key="join" class="panel form join" aria-label="Join game">
        <button class="back" @click="goBack" aria-label="Back to menu">Back</button>
        <label for="name-join">Name</label>
        <input
          id="name-join"
          v-model="name"
          type="text"
          placeholder="Your name"
          autocomplete="name"
        />
        <label for="room">Room code</label>
        <input
          id="room"
          v-model="roomCode"
          type="text"
          placeholder="ABCD"
          autocapitalize="characters"
          inputmode="text"
          @keyup.enter="onJoin"
        />
        <button class="cta primary" :disabled="!canJoin" @click="onJoin">Join</button>
      </section>
    </Transition>
  </div>
</template>

<style scoped>
.start-flow {
  width: min(520px, 92vw);
  margin: 0 auto;
}
.panel {
  background: rgba(15, 23, 42, 0.5);
  border: 1px solid rgba(148, 163, 184, 0.25);
  border-radius: 16px;
  padding: 24px;
  display: grid;
  gap: 16px;
}
.menu {
  place-items: center;
}
.form {
  position: relative;
  /* Reserve space at the top so the absolutely-positioned Back button doesn't overlap labels/inputs */
  padding-top: 56px;
}
.back {
  position: absolute;
  top: 12px;
  left: 12px;
  background: transparent;
  border: 1px solid rgba(148, 163, 184, 0.35);
  color: #e2e8f0;
  padding: 10px 14px;
  border-radius: 10px;
  font-size: 1rem;
  line-height: 1.1;
  min-height: 40px;
  cursor: pointer;
}
label {
  font-size: 0.9rem;
  color: #cbd5e1;
}
input {
  background: #0b1220;
  color: #e2e8f0;
  border: 1px solid #334155;
  border-radius: 8px;
  padding: 10px 12px;
  outline: none;
}
input:focus {
  border-color: #22c55e;
  box-shadow: 0 0 0 2px rgba(34, 197, 94, 0.25);
}

.cta {
  width: 100%;
  padding: 12px 16px;
  font-weight: 700;
  border-radius: 10px;
  cursor: pointer;
  transition: transform 0.05s ease-in-out, background 0.2s ease, color 0.2s ease, border-color 0.2s ease;
}
.cta:active { transform: translateY(1px); }
.primary {
  background: #22c55e;
  color: #08230f;
  border: 1px solid #16a34a;
}
.outline {
  background: transparent;
  color: #22c55e;
  border: 1px solid #16a34a;
}
.cta:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.or {
  text-transform: uppercase;
  color: #94a3b8;
  letter-spacing: 0.08em;
}

/* Transitions */
.fade-enter-active, .fade-leave-active { transition: opacity 200ms ease; }
.fade-enter-from, .fade-leave-to { opacity: 0; }

.slide-down-enter-active, .slide-down-leave-active {
  transition: transform 260ms ease, opacity 260ms ease;
}
.slide-down-enter-from, .slide-down-leave-to { transform: translateY(-12px); opacity: 0; }

.slide-up-enter-active, .slide-up-leave-active {
  transition: transform 260ms ease, opacity 260ms ease;
}
.slide-up-enter-from, .slide-up-leave-to { transform: translateY(12px); opacity: 0; }
</style>
