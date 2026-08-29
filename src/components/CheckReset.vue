<template>
  <section class="check-panel">
    <button
      type="button"
      class="check-button"
      @click="generateRandom"
    >
      Check
    </button>

    <div v-if="checked" class="result">
      Error: {{ errorPercentage.toFixed(1) }}%
    </div>

    <button
      type="button"
      class="reset-button"
      @click="resetPage"
    >
      Reset
    </button>
  </section>
</template>

<script setup>
import { computed, ref } from 'vue'

const checked = ref(false)

const props = defineProps({
  firstArray: {
    type: Array,
    required: true
  },
  secondArray: {
    type: Array,
    required: true
  }
})

const emit = defineEmits(['random'])

function resetPage() {
  window.location.reload()
}

const errorPercentage = computed(() => {
  if (props.firstArray.length !== props.secondArray.length) {
    return 100
  }

  const first = [...props.firstArray].sort((a, b) => a - b)
  const second = [...props.secondArray].sort((a, b) => a - b)

  if (first.length === 0) {
    return 0
  }

  const totalRelativeError = first.reduce((total, value, index) => {
    const target = second[index]

    if (target === 0) {
      return total + (value === 0 ? 0 : 1)
    }

    return total + Math.abs(value - target) / Math.abs(target)
  }, 0)

  return Math.min(100, (totalRelativeError / first.length) * 100)
})

const arraysAreEqual = computed(() => errorPercentage.value === 0)

function generateRandom() {
  checked.value = true

  const randomNumber = Math.floor(Math.random() * 10000) + 1

  emit('random', randomNumber)
}
</script>

<style scoped>
button {
  width: 100%;
  min-height: 56px;
  border: none;
  border-radius: 12px;
  background: #333;
  color: white;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  touch-action: manipulation;
}

button:active {
  transform: scale(0.98);
}

div {
  margin-top: 1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 52px;
  padding: 0.75rem 1rem;
  box-sizing: border-box;
  border-radius: 12px;
  background: #f5f5f5;
  color: #333;
  text-align: center;
  font-size: 1.15rem;
  font-weight: 600;
}

.reset-button {
  width: 100%;
  min-height: 52px;
  border: 1px solid #ddd;
  border-radius: 12px;
  background: #f5f5f5;
  color: #333;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  touch-action: manipulation;
  transition: transform 0.15s ease, background 0.15s ease;
}

.reset-button:active {
  transform: scale(0.98);
  background: #e9e9e9;
}

@media (max-width: 480px) {
  .check-panel {
    padding: 0.85rem;
    border-radius: 12px;
  }

  .check-button,
  .reset-button {
    min-height: 56px;
  }
}
</style>


