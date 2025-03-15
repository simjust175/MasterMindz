<template>
    <v-btn :disabled="dis" icon="mdi-check"></v-btn>
    <v-btn  icon="mdi-close" @click="dis = !dis"></v-btn>
</template>

<script setup>
import { ref } from "vue"
const dis = ref(false)

</script>