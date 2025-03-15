<template>
  <v-card
    height="100"
    width="380"
    class="pl-4 d-flex align-center"
    rounded="0"
    :color="activeRow !== rowId ? 'grey-lighten-2' : 'blue-lighten-5'"
    :disabled="activeRow !== rowId"
    :loading="verifyingRow"
  >
    <!-- Row Number -->
    <h3 class="mr-4 text-grey-lighten-1">
      {{ rowId }}
    </h3>
    
    <validate-choice
      :players-choice="playersChoice"
      :winning-colors="props.winningColors"
      :missing-pegs="missingPegsProps"
      :loading-active="verifyingRow"
      @results="finishTurn"
    />

    <!-- 3D Dome Buttons -->
    <v-btn
      v-for="button in 4"
      :id="`button${button}`"
      :key="button"
      :class="{'dome-button-white': playersChoice[button] === 'white', 'dome-button-other': playersChoice[button]}"
      :color="playersChoice[button] ? playersChoice[button] : 'grey-lighten-1'"
      class="mr-1"
      icon
      @click="triggerPicker(`${button}${props.row}`)"
    />

    <color-picker
      :activate="triggerColorPicker"
      :index="currentButton"
      @color-choice="setColor($event, currentButton)"
    />
    <result-pegs :results-colors="resultsColors" />
  </v-card>
</template>

<script setup>
import { ref, computed } from "vue";

const props = defineProps({
  row: Number,
  activeRow: Number,
  winningColors: Array,
});

const emit = defineEmits(["rowUpdate", "winner"]);
const triggerColorPicker = ref(false);
const currentButton = ref("");

const rowId = computed(() => props.row);
const activeRow = computed(() => props.activeRow);

const verifyingRow = ref(false);
const resultsColors = ref({});
const missingPegs = ref(true);
const missingPegsProps = computed(() => missingPegs.value);
const playersChoice = ref({ 1: "", 2: "", 3: "", 4: "" });

const triggerPicker = (index) => {
  currentButton.value = index;
  triggerColorPicker.value = !triggerColorPicker.value;
};

const setColor = (event, index) => {
  triggerColorPicker.value = false;
  playersChoice.value[index[0]] = event;
  if (Object.values(playersChoice.value).every((peg) => peg))
    missingPegs.value = false;
};

const finishTurn = (event) => {
  if (event.white === 4) return emit("winner");
  verifyingRow.value = 'green';
  setTimeout(() => {
    verifyingRow.value = false;
    resultsColors.value = event;
    emit("rowUpdate");
  }, 1000);
};
</script>

<style scoped>
/* 🏆 3D Dome Button Effect */
.dome-button-white {
  /* background: linear-gradient(to bottom, #ffffff 10%, #e0e0e0 50%, #b0b0b0 100%); */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2), inset 0 2px 4px rgba(216, 216, 216, 0.466);
}
.dome-button-other {
  /* background: linear-gradient(to bottom, #ffffff 10%, #e0e0e0 50%, #b0b0b0 100%); */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2), inset 0 2px 4px rgba(255, 255, 255, 0.715);
}

/* Pressed Effect */
.dome-button:active {
  /* background: linear-gradient(to bottom, #b0b0b0 10%, #909090 50%, #606060 100%); */
  box-shadow: inset 0 3px 6px rgba(0, 0, 0, 0.3);
  transform: scale(0.95);
}
</style>

