<template>
  <div
    class="relative flex flex-col overflow-hidden space-y-2"
    :class="expand ? 'h-full' : 'max-h-32'"
  >
    <slot name="body"></slot>
    <div
      class="sticky inset-x-0 bottom-0 flex items-center justify-center flex-shrink-0 overflow-x-auto"
    >
      <HoppButton
        :icon="expand ? IconChevronUp : IconChevronDown"
        :label="
          expand
            ? less ?? t?.('action.less') ?? 'Less'
            : more ?? t?.('action.more') ?? 'More'
        "
        filled
        rounded
        @click="expand = !expand"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { HoppButton } from "../button"
import IconChevronUp from "~icons/lucide/chevron-up"
import IconChevronDown from "~icons/lucide/chevron-down"
import { inject, ref } from "vue"
import { HoppUIPluginOptions, HOPP_UI_OPTIONS } from "./../../plugin"

const { t } = inject<HoppUIPluginOptions>(HOPP_UI_OPTIONS) ?? {}

const expand = ref(false)

withDefaults(
  defineProps<{
    less?: string
    more?: string
  }>(),
  {
    less: "Less",
    more: "More",
  },
)
</script>
