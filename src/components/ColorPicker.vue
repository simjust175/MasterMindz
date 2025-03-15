<template>
  <v-speed-dial
    id="level"
    v-model="activatorProps"
    location="right center"
    transition="fade-transition"
    z-index="9999"
    :attach="currentButton"
    close-on-content-click
  >
    <v-btn key="1" color="red" icon @click.stop="emitColor('red')" />
    <v-btn key="2" color="blue" icon @click.stop="emitColor('blue')" />
    <v-btn key="3" color="yellow" icon @click.stop="emitColor('yellow')" />
    <v-btn key="4" color="green" icon @click.stop="emitColor('green')" />
    <v-btn key="5" color="black" icon @click.stop="emitColor('black')" />
    <v-btn key="6" color="white" icon @click.stop="emitColor('white')" />
  </v-speed-dial>
</template>

<script setup>
import { ref, computed, watch } from "vue";
const props = defineProps({
  activate: Boolean,
  // eslint-disable-next-line vue/require-default-prop
  index: Number,
});
const currentButton = computed(() => `button#button${props.index}`);
const emit = defineEmits(["colorChoice"]);
const activatorProps = ref(false);
watch(
  () => props.activate,
  (newVal) => (activatorProps.value = newVal)
);
const emitColor = (color) => {
  emit("colorChoice", color, props.index);
};
</script>

<style>
#level {
  z-index: 999 !important;
}
</style>
