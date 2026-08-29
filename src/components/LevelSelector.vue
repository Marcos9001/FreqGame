<template>
  <section class="level-selector">
    <h2>Select Level</h2>

    <div class="level-buttons">
      <button
        v-for="level in levels"
        :key="level"
        type="button"
        :class="{ active: selectedLevel === level }"
        @click="selectLevel(level)"
      >
        Level {{ level }}
      </button>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'

const levels = [1, 2, 3]

const selectedLevel = ref(1)

const emit = defineEmits(['level-change'])

function selectLevel(level) {
  selectedLevel.value = level

  emit('level-change', level)
}
</script>

<style scoped>
.level-selector {
  width: 100%;
  box-sizing: border-box;
  padding: 1rem;
  border-radius: 16px;
  background: #fff;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
}

.level-selector h2 {
  margin: 0 0 1rem;
  font-size: 1.2rem;
}

.level-buttons {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 0.5rem;
}

.level-buttons button {
  min-height: 48px;
  padding: 0.75rem 0.5rem;
  border: 1px solid #ddd;
  border-radius: 10px;
  background: #f5f5f5;
  color: #333;
  font-size: 0.95rem;
  font-weight: 500;
  cursor: pointer;
  touch-action: manipulation;
  transition: 0.2s ease;
}

.level-buttons button:active {
  transform: scale(0.97);
}

.level-buttons button.active {
  background: #333;
  color: #fff;
  border-color: #333;
}

@media (max-width: 480px) {
  .level-selector {
    padding: 0.85rem;
    border-radius: 12px;
  }

  .level-selector h2 {
    font-size: 1.05rem;
  }

  .level-buttons {
    gap: 0.4rem;
  }

  .level-buttons button {
    min-height: 52px;
    font-size: 0.9rem;
  }
}
</style>
```
