<template>
  <v-btn
    icon="mdi-check-circle"
    color="success"
    density="comfortable"
    class="mr-4"
    :disabled="props.missingPegs"
    :loading="loadingActive"
  />
</template>

<script setup>
const props = defineProps({
  // eslint-disable-next-line vue/require-default-prop
  playersChoice: Object,
  // eslint-disable-next-line vue/require-default-prop
  winningColors: Object,
  missingPegs: Boolean,
  loadingActive: Boolean
});

const emit = defineEmits(["results"]);
const validatePlayersChoice = () => {
  const computer = Object.values(props.winningColors);
  const player = Object.values(props.playersChoice);

  let whites = 0; //Matching color and position
  let reds = 0; //Matching color but wrong position

  for (let i = 0; i <= 3; i++) {
      if (computer[i] === player[i]) {
        whites++;
        computer[i] = "";
        player[i]="";
      } 
    }
  //after we search for 'whites' we look for 'reds'
    player.forEach(peg => {
        if (peg && computer.includes(peg)) {
            console.log("we're here:", computer, peg);
            computer[computer.indexOf(peg)] = ""
            reds++};
    })
  emit("results", { white: whites, red: reds });
};

const checkResults = () => {
  if (!Object.values(props.playersChoice).every((peg) => peg)) return null; //extra validation that all pegs have been chosen
  validatePlayersChoice();
};
</script>
