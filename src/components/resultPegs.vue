<template>
  <v-container>
    <v-row no-gutters>
      <v-col
        v-for="button in 2"
        :key="'btn-' + button"
        cols="6"
      >
        <v-btn
          :color="resultsColorSetup(button -1)"
          rounded="xl"
          density="compact"
          size="small"
          icon
        />
      </v-col>
    </v-row>

    <v-row no-gutters>
      <v-col
        v-for="button in 2"
        :key="'btn-' + (button)"
        cols="6"
      >
        <v-btn
          rounded="xl"
          density="compact"
          size="small"
          :color="resultsColorSetup(button + 1)"
          icon
        />
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import { ref, watch } from "vue";

const props = defineProps({
  // eslint-disable-next-line vue/require-default-prop
  resultsColors: Object,
});

const results = ref([]);

const colorPegs = (colors) => {
  if (!colors) return;
  results.value = [];

  Object.entries(colors).forEach(([color, count]) => {
    for (let i = 0; i < count; i++) {
      results.value.push(color);
    }
  });
  results.value = results.value.sort()
};

const resultsColorSetup = (index) => results.value[index] || "grey-darken-1";


watch(
  () => props.resultsColors,
  (newColors) => colorPegs(newColors),
  { deep: true }
);
</script>
