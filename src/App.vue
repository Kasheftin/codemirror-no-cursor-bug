<template>
  <div>
    <div>codemirror raw: there's a blinking cursor when the content is empty</div>
    <div ref="editorRawRef" />
    <div>vue-codemirror: no cursor when the content is empty</div>
    <Codemirror v-model="code" :extensions="[oneDark]" />
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, watch } from 'vue'
import { Codemirror } from 'vue-codemirror'
import { oneDark } from '@codemirror/theme-one-dark';

import { EditorView, basicSetup } from 'codemirror'

const code = ref('')

const editorRawRef = ref<HTMLDivElement>()

let editorRaw: EditorView | undefined = undefined

onMounted(() => {
  if (editorRawRef.value) {
    editorRaw = new EditorView({
      parent: editorRawRef.value,
      extensions: [basicSetup, EditorView.updateListener.of((viewUpdate) => {
        if (viewUpdate.docChanged) {
          code.value = viewUpdate.state.doc.toString()
        }
      }), oneDark]
    })
  }
})

watch(code, (value) => {
  if (editorRaw) {
    if (value !== editorRaw.state.doc.toString()) {
      editorRaw.dispatch({
        changes: {
          from: 0,
          to: editorRaw.state.doc.length,
          insert: value
        }
      })
    }
  }
})

</script>
