<template>
  <section class="sound-panel">
    <button
      type="button"
      class="start-button"
      @click="playSounds"
    >
      Start
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
  frequencies: {
    type: Array,
    required: true
  }
})

const isPlaying = ref(false)

let audioContext = null
let oscillators = []
let gainNode = null

function playSounds() {
  if (isPlaying.value) {
    return
  }

  // Create the audio context
  audioContext = new (window.AudioContext || window.webkitAudioContext)()

  // Create one gain node for all oscillators
  gainNode = audioContext.createGain()

  // Keep the volume relatively low
  gainNode.gain.value = 0.1

  gainNode.connect(audioContext.destination)

  // Create one oscillator for every frequency.
  oscillators = props.frequencies.map((frequency) => {
    const oscillator = audioContext.createOscillator()

    oscillator.type = 'sine'
    oscillator.frequency.value = frequency

    oscillator.connect(gainNode)
    oscillator.start()

    return oscillator
  })

  isPlaying.value = true
}

// Update the existing oscillators whenever the sliders change.
// We do NOT stop/recreate them, which avoids clicks and glitches.
watch(
  () => props.frequencies,
  (frequencies) => {
    if (!isPlaying.value) {
      return
    }

    frequencies.forEach((frequency, index) => {
      if (oscillators[index]) {
        // A small ramp prevents an audible click when changing frequency.
        const now = audioContext.currentTime

        oscillators[index].frequency.cancelScheduledValues(now)
        oscillators[index].frequency.setValueAtTime(
          oscillators[index].frequency.value,
          now
        )
        oscillators[index].frequency.linearRampToValueAtTime(
          frequency,
          now + 0.03
        )
      }
    })
  },
  { deep: true }
)

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
