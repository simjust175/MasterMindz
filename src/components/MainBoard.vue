<template>
  <v-container class="bg-grey-lighten-1">
    <winner-dialog :activateDialog="winner" />
    <v-row v-for="row in 12" :key="row">
      <row-component
        :row="row"
        :active-row="activeRow"
        :winning-colors="winningColors"
        @row-update="activeRow++"
        @winner="winner = true"
      />
    </v-row>
  </v-container>
</template>

<script setup>
import { ref, reactive } from "vue";

const colors = ["red", "yellow", "green", "blue", "black", "white"];
const activeRow = ref(1);
const winningColors = reactive({ 1: "", 2: "", 3: "", 4: "" });

const setWinningNumbers = () => {
  for (let index = 0; index < 4; index++) {
    const randomNumber = Math.ceil(Math.random() * 5);
    winningColors[index + 1] = colors[randomNumber];
  }
  console.log(Object.values(winningColors));
};

setWinningNumbers();
const winner = ref(false);
</script>

<style></style>
