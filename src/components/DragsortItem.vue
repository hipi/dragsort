<template>
  <div
    class="fc-dragsort-item"
    draggable="isDraggable"
    :class="itemClass"
    :style="fwStyle"
    @dragstart="handleDragstart"
    @drag="handleDrag"
    @dragenter="handleDragover"
    @dragend="handleDragend"
    ref="itemRef"
  >
    <div class="fc-dragsort-item__container">
      <slot></slot>
    </div>
  </div>
</template>
<script>
function swap(arr, dragKey, targetKey) {
  const newArr = [...arr]

  const dIndex = newArr.findIndex((item) => JSON.stringify(item) === dragKey)
  const tIndex = newArr.findIndex((item) => JSON.stringify(item) === targetKey)
  const d_data = newArr[dIndex]
  const t_data = newArr[tIndex]
  newArr[dIndex] = t_data
  newArr[tIndex] = d_data
  return newArr
}
export default {
  props: {
    item: {
      type: Number | String | Object,
    },
  },
  inject: ['dragsort'],
  data() {
    return {
      isDraggable: true,
      isDraging: false,
    }
  },
  computed: {
    fwStyle() {
      const basis = `${100 / this.dragsort.colum}%`
      return {
        maxWidth: basis,
        flex: `0 0 ${basis}`,
      }
    },
    itemClass() {
      return {
        'is-draging': this.isDraging,
        ghost: this.isDraging,
      }
    },
  },
  methods: {
    handleDragstart() {
      this.isDraging = true
      this.dragsort.dragEl = this.$refs.itemRef
      this.dragsort.dragKey = JSON.stringify(this.item)
    },
    handleDrag() {
      console.log('handleDrag')
    },
    handleDragend() {
      this.isDraging = false
      this.dragsort.dragEl = null
      this.dragsort.targetEl = null
    },
    handleDragover(e) {
      const dragEl = this.dragsort.dragEl
      const targetEl = this.$refs.itemRef
      const dragKey = this.dragsort.dragKey
      const targetKey = JSON.stringify(this.item)

      if (dragKey !== targetKey) {
        // 替换 加 动画
        let sortData = JSON.parse(JSON.stringify(this.dragsort.sortData))
        this.dragsort._setNewSortData(swap(sortData, dragKey, targetKey))
      }
    },
  },
}
</script>
