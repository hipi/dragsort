<template>
  <div class="fc-dragsort-item" draggable="isDraggable">
    <div class="fc-dragsort-item__container">
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isDraggable: true,
    }
  },
}
</script>
