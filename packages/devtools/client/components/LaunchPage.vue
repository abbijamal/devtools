<script setup lang="ts">
import type { ModuleLaunchAction } from '~~/../src/types'
import { telemetry } from '~/composables/telemetry'

const props = defineProps<{
  name?: string
  icon?: string
  title?: string
  description?: string
  actions?: ModuleLaunchAction[]
}>()

const emits = defineEmits<{
  (e: 'action', idx: number): void
}>()

function launch(action: ModuleLaunchAction, idx: number) {
  telemetry(`launch-page`, {
    pageName: props.name,
  })
  action.handle?.()
  emits('action', idx)
}
</script>

<template>
  <NPanelGrids>
    <NCard flex="~ col gap2" mxa min-w-100 items-center p6>
      <TabIcon mb2 text-5xl :icon="icon || icon" :title="title" />
      <h1 text-xl>
        {{ title }}
      </h1>
      <NMarkdown v-if="description" mb2 mt--1 text-center text-base op50 :markdown="description" />
      <div flex="~ gap2 wrap">
        <template v-for="action, idx of actions" :key="idx">
          <NButton
            n="solid primary"
            :disabled="action.pending"
            :to="action.src"
            :target="action.src ? '_blank' : undefined"
            v-bind="action.attrs"
            @click="launch(action, idx)"
          >
            <NIcon v-if="action.pending" icon="carbon-circle-dash" animate-spin />
            {{ action.label }}
          </NButton>
        </template>
      </div>
    </NCard>
  </NPanelGrids>
</template>
