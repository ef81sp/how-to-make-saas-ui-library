<script setup lang="ts">
import { computed } from "vue";

const {
  captions,
  at = "+1",
} = defineProps<{
  captions: string[];
  at?: string | number;
}>();

const captionsData = computed(() => {
  return captions.map((caption, index) => ({
    text: caption,
    slotName: (index + 1).toString(),
  }));
});
</script>

<template>
  <div aria-live="polite">
    <VSwitch :at>
      <template
        v-for="(caption, index) in captionsData"
        :key="index"
        v-slot:[caption.slotName]
      >
        <span v-html="caption.text"></span>
      </template>
    </VSwitch>
  </div>
</template>

<style scoped>
:deep(.slidev-vclick-target) {
  transition: opacity 0.2s ease-in-out;
}

:deep(.slidev-vclick-hidden) {
  opacity: 0;
  pointer-events: none;
}

/* 切替時に一瞬何も表示されない時間を作る */
:deep(.slidev-vclick-target:not(.slidev-vclick-hidden)) {
  animation: appear-with-gap 0.2s ease-in-out;
}

@keyframes appear-with-gap {
  0% {
    opacity: 0;
    visibility: hidden;
  }
  50% {
    opacity: 0;
    visibility: hidden;
  }
  51% {
    opacity: 0;
    visibility: visible;
  }
  100% {
    opacity: 1;
    visibility: visible;
  }
}
</style>
