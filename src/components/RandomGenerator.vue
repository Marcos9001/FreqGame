<template>
  <section class="sound-panel">
    <button
      type="button"
      class="start-button"
      @click="playSounds"
    >
      Play
    </button>

    <button
      type="button"
      class="stop-button"
      @click="stopSounds"
    >
      Stop
    </button>
  </section>
</template>

<script setup>
import { ref, watch, onBeforeUnmount } from 'vue'

const props = defineProps({
  level: {
    type: Number,
    required: true
  },
  seed: {
    type: Number,
    required: true
  }
})

const emit = defineEmits(['frequencies-generated'])

const isPlaying = ref(false)

let audioContext = null
let oscillators = []
let gainNode = null

const MIN_FREQUENCY = 250
const MAX_FREQUENCY = 800

// Seeded pseudo-random number generator.
// The same seed always produces the same sequence of frequencies.
function seededRandom(seed) {
  let value = seed

  return () => {
    value = (value * 9301 + 49297) % 233280
    return value / 233280
  }
}

function generateFrequencies() {
  const count = Math.min(Math.max(Math.floor(props.level), 1), 3)
  const random = seededRandom(props.seed)

  return Array.from({ length: count }, () => {
    return Math.floor(
      random() * (MAX_FREQUENCY - MIN_FREQUENCY + 1) + MIN_FREQUENCY
    )
  })
}

function playSounds() {
  if (isPlaying.value) {
    return
  }

  const frequencies = generateFrequencies()

  emit('frequencies-generated', frequencies)

  // Create the audio context
  audioContext = new (window.AudioContext || window.webkitAudioContext)()

  // Create one gain node for all oscillators
  gainNode = audioContext.createGain()

  // Keep the volume relatively low
  gainNode.gain.value = 0.1

  gainNode.connect(audioContext.destination)

  // Create one oscillator for every frequency.
  oscillators = frequencies.map((frequency) => {
    const oscillator = audioContext.createOscillator()

    oscillator.type = 'sine'
    oscillator.frequency.value = frequency

    oscillator.connect(gainNode)
    oscillator.start()

    return oscillator
  })

  isPlaying.value = true
}

function stopSounds() {
  oscillators.forEach((oscillator) => {
    oscillator.stop()
    oscillator.disconnect()
  })

  oscillators = []

  if (gainNode) {
    gainNode.disconnect()
    gainNode = null
  }

  if (audioContext) {
    audioContext.close()
    audioContext = null
  }

  isPlaying.value = false
}

onBeforeUnmount(() => {
  stopSounds()
})
</script>

<style scoped>
.sound-panel {
  width: 100%;
  display: flex;
  justify-content: center;
  padding: 1rem;
  box-sizing: border-box;
}

.start-button,
.stop-button {
  flex: 1;
  min-height: 52px;
  border: none;
  border-radius: 12px;
  color: white;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  touch-action: manipulation;
}

.start-button {
  background: #333;
}

.stop-button {
  background: #777;
}

.start-button:active,
.stop-button:active {
  transform: scale(0.98);
}

@media (max-width: 480px) {
  .start-button,
  .stop-button {
    min-height: 56px;
  }
}
</style>
