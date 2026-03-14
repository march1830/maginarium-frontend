<template>
  <main class="container">
    <header class="header">
      <img alt="Maginarium" src="/favicon.svg" class="logo" />
      <h1>Maginarium</h1>
      <p class="tagline">Start a new game or join an existing room</p>
    </header>

    <StartFlow v-if="view === 'start'" @create="onCreate" @join="onJoin" />

    <RoomLobby
      v-else
      :room-code="roomCode"
      :players="players"
      :me="me"
      @startGame="onStartGame"
      @leave="onLeaveRoom"
    />
  </main>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import StartFlow from './components/StartFlow.vue'
import RoomLobby from './components/RoomLobby.vue'

type View = 'start' | 'lobby'
const view = ref<View>('start')

// Demo state (will be replaced by real backend integration later)
const roomCode = ref('ABCD')
const me = ref<string>('')
const players = ref<Array<{ name: string; isAdmin?: boolean }>>([])

function onCreate(payload: { name: string }) {
  me.value = payload.name
  roomCode.value = 'NEW1'
  players.value = [
    { name: payload.name, isAdmin: true },
    { name: 'Jamie' },
    { name: 'Riley' }
  ]
  view.value = 'lobby'
}

function onJoin(payload: { name: string; roomCode: string }) {
  me.value = payload.name
  roomCode.value = payload.roomCode
  players.value = [
    { name: 'Alex', isAdmin: true },
    { name: payload.name },
    { name: 'Sam' }
  ]
  view.value = 'lobby'
}

function onStartGame() {
  // Placeholder action; replace with navigation to game screen later
  console.log('Starting game for room', roomCode.value)
  alert(`Starting game in room ${roomCode.value}! (demo)`) // eslint-disable-line no-alert
}

function onLeaveRoom() {
  // Clear demo state when leaving
  me.value = ''
  players.value = []
  roomCode.value = 'ABCD'
  view.value = 'start'
}
</script>

<style scoped>
.container {
  display: grid;
  grid-template-rows: auto 1fr;
  gap: 2rem;
  min-height: 100vh;
  place-items: center;
  padding: 2rem;
}
.header {
  text-align: center;
}
.logo {
  height: 64px;
}
.tagline {
  color: #64748b;
}
</style>
