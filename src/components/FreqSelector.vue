<template>
  <section class="slider-panel">
    <div class="panel-header">
      <h2>Select Frequencies</h2>
      <span class="level-label">Level {{ level }}</span>
    </div>

    <div class="sliders">
      <div
        v-for="(value, index) in sliderValues"
        :key="index"
        class="slider-row"
      >
        <div class="slider-info">
          <label :for="`slider-${index}`">
            Freq: {{ index + 1 }}
          </label>

          <span class="slider-value">
            {{ value }}
          </span>
        </div>

        <input
          :id="`slider-${index}`"
          v-model.number="sliderValues[index]"
          type="range"
          :min="MIN_VALUE"
          :max="MAX_VALUE"
          :step="STEP"
          class="slider"
        />

        <div class="slider-range">
          <span>{{ MIN_VALUE }}</span>
          <span>{{ MAX_VALUE }}</span>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { computed, ref, watch } from 'vue'

// The level comes from the parent component.
const props = defineProps({
  level: {
    type: Number,
    required: true
  }
})

// Slider configuration
const MIN_VALUE = 250
const MAX_VALUE = 800
const STEP = 1

// Number of sliders based on the selected level.
// Level 1 -> 1 slider
// Level 2 -> 2 sliders
// Level 3 -> 3 sliders
const sliderCount = computed(() => {
  const level = Math.max(1, Math.floor(props.level))

  return Math.min(level, 3)
})

// Store the value of every slider.
const sliderValues = ref([])

// Initialize the sliders.
function initializeSliders() {
  const count = sliderCount.value
  const oldValues = sliderValues.value

  sliderValues.value = Array.from(
    { length: count },
    (_, index) => oldValues[index] ?? 50
  )
}

// Initial setup
initializeSliders()

// Reinitialize when the level changes.
watch(sliderCount, () => {
  initializeSliders()
})

// Calculate one computed value for each slider.
// Replace this formula with the calculation you need.
const computedValues = computed(() => {
  return sliderValues.value.map((value) => {
    return value
  })
})

const emit = defineEmits(['sliders-change'])

watch(
  sliderValues,
  (values) => {
    emit('sliders-change', values)
  },
  { deep: true, immediate: true }
)
</script>

<style scoped>
.slider-panel {
  width: 100%;
  box-sizing: border-box;
  padding: 1rem;
  border-radius: 16px;
  background: #fff;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
}

.panel-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  margin-bottom: 1.5rem;
}

.panel-header h2 {
  margin: 0;
  font-size: 1.2rem;
}

.level-label {
  padding: 0.35rem 0.7rem;
  border-radius: 999px;
  background: #f1f1f1;
  font-size: 0.85rem;
  white-space: nowrap;
}

.sliders {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.slider-row {
  width: 100%;
}

.slider-info {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  margin-bottom: 0.5rem;
}

.slider-info label {
  font-size: 0.95rem;
  font-weight: 500;
}

.slider-value {
  min-width: 3rem;
  text-align: right;
  font-weight: 600;
}

.slider {
  display: block;
  width: 100%;
  height: 2rem;
  margin: 0;
  cursor: pointer;
  accent-color: #444;
  touch-action: manipulation;
}

.slider-range {
  display: flex;
  justify-content: space-between;
  color: #888;
  font-size: 0.75rem;
}

.result {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  margin-top: 1.5rem;
  padding: 1rem;
  border-radius: 12px;
  background: #f5f5f5;
}

.result-label {
  font-size: 0.9rem;
  color: #666;
}

.result strong {
  font-size: 1.25rem;
}

@media (max-width: 480px) {
  .slider-panel {
    padding: 0.85rem;
    border-radius: 12px;
  }

  .panel-header {
    margin-bottom: 1.25rem;
  }

  .panel-header h2 {
    font-size: 1.05rem;
  }

  .sliders {
    gap: 1rem;
  }

  .slider {
    height: 2.5rem;
  }

  .result {
    margin-top: 1.25rem;
    padding: 0.85rem;
  }
}
</style>

