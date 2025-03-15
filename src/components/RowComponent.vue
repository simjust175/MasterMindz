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
    <!-- :variant="selectedRowVariant" -->
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

    <v-btn
      v-for="button in 4"
      :id="`button${button}`"
      :key="button"
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
  // eslint-disable-next-line vue/require-default-prop
  row: Number,
  // eslint-disable-next-line vue/require-default-prop
  activeRow: Number,
  // eslint-disable-next-line vue/require-default-prop
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
  verifyingRow.value = true;
  setTimeout(() => {
    verifyingRow.value = false;
    resultsColors.value = event;
    emit("rowUpdate");
  }, 1000);
 
};

</script>
