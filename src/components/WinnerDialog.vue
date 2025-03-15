<template>
  <v-dialog v-model="activate" max-width="500">
    <v-card rounded="lg">
      <v-card-title class="d-flex justify-space-between align-center">
        <div class="text-h5 text-medium-emphasis ps-2"> Congratulations!!! </div>

        <v-btn
          icon="mdi-close"
          variant="text"
          @click="isActive.value = false"
        />
      </v-card-title>

      <v-divider class="mb-4" />

      <v-card-text>
        <div class="text-medium-emphasis mb-4"> You WON!!! </div>

        <!-- <v-btn
            class="text-none font-weight-bold ms-n4"
            color="success"
            text="New game"
            variant="text"
          /> -->
      </v-card-text>

      <v-divider class="mt-2" />

      <v-card-actions class="my-2 d-flex justify-end">
        <v-btn
          class="text-none"
          rounded="xl"
          text="Cancel"
          @click="activate = false"
        />

        <v-btn
          class="text-none px-2"
          color="success"
          rounded="xl"
          text="New game"
          variant="flat"
          @click="activate = false"
        />
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script setup>
import confetti from 'https://esm.run/canvas-confetti@1'
import { ref, watch } from "vue";
const props = defineProps({
  activateDialog: Boolean,
});
const activate = ref(false);
watch(()=> props.activateDialog, ()=> {
    activate.value = true;
    confetti({ particleCount: 500, spread: 160 })
    setTimeout(() => {
        confetti({ particleCount: 0, spread: 0 })
    }, 1000);
})
</script>
