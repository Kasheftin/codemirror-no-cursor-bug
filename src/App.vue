<template>
  <div>
    <Codemirror v-model="code" :extensions="extensions" />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { Codemirror } from 'vue-codemirror'
import { oneDark } from '@codemirror/theme-one-dark'
const code = ref('')
const extensions = [oneDark]
</script>
