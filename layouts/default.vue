<template>
  <div class="slidev-layout default">
    <!-- デフォルトレイアウトの内容をベースにカスタマイズ -->
    <slot name="default" />
    <div class="captions-container">
      <slot name="captions" />
    </div>
  </div>
</template>

<style scoped>
.captions-container {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: light-dark(black, #1a1a1a); /* Darker background for dark mode */
}

.captions-container:deep(p) {
  color: light-dark(white, #e0e0e0); /* Lighter text for dark mode */
}
.captions-container:deep(code) {
  background-color: light-dark(lightgray, #333333); /* Darker code background for dark mode */
  color: light-dark(maroon, #ff6666); /* Lighter code text for dark mode */
}
</style>
