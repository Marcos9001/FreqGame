<script setup>

import { ref } from 'vue'
import LevelSelector from './components/LevelSelector.vue';
import FreqSelector from './components/FreqSelector.vue';
import SlidersSpeaker from './components/SlidersSpeaker.vue';
import RandomGenerator from './components/RandomGenerator.vue';
import CheckReset from './components/CheckReset.vue';

const selectedLevel = ref(1)
const sliderValues = ref([])
const randomNumber = ref(0)
const frequenciesRandom = ref([])


</script>

<template>
  <LevelSelector
    @level-change="selectedLevel = $event"
  />

  <RandomGenerator
    :level="selectedLevel"
    :seed="randomNumber"
    @frequencies-generated="frequenciesRandom = $event"
  />

  <FreqSelector
    :level="selectedLevel"
    @sliders-change="sliderValues = $event"
  />

  <SlidersSpeaker
    :frequencies="sliderValues"
  />

  <CheckReset
    @random="randomNumber = $event"
    :first-array="sliderValues"
    :second-array="frequenciesRandom"
  />

</template>