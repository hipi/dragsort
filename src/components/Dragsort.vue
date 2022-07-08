<template>
  <div class="fc-dragsort" @dragover="handleDragover">
    <slot />
  </div>
</template>

<script>
export default {
  name: 'FcDragsort',
  provide() {
    return {
      dragsort: this,
    }
  },
  data() {
    return {}
  },
  methods: {
    handleDragover(e) {
      e.preventDefault()
    },
  },
}
</script>
