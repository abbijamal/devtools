<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{ id?: string }>()

const ext = computed(() => props.id?.split(/\./g).pop() || '')
const icon = computed(() => {
  switch (ext.value) {
    case 'vue':
      return 'logos-vue'
    default:
      return 'carbon-document-blank'
  }
})
</script>

<template>
  <div :class="icon" />
</template>
