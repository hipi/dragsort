<template>
  <div
    class="fc-dragsort-item"
    draggable="isDraggable"
    :class="itemClass"
    :style="fwStyle"
    @dragstart="handleDragstart"
    @dragover="handleDragover"
    @dragend="handleDragend"
    ref="itemRef"
  >
    <div class="fc-dragsort-item__container">
      <slot></slot>
    </div>
  </div>
</template>
<script>
function sortArr(arr, dragId, targeId) {
  const newArr = [...arr]
  const dIndex = newArr.findIndex((item) => item._id === dragId)
  const tIndex = newArr.findIndex((item) => item._id === targeId)
  const dValue = newArr[dIndex]
  if (dIndex < tIndex) {
    // 排在 tIndex 后面
    newArr.splice(tIndex + 1, 0, dValue)
    newArr.splice(dIndex, 1)
  } else {
    // 排在 tIndex 前面
    newArr.splice(tIndex, 0, dValue)
    newArr.splice(dIndex + 1, 1)
  }
  return newArr
}
export default {
  props: {
    _id: {
      type: String,
    },
    index: {
      type: Number,
    },
    item: {
      type: Number | String | Object,
    },
  },
  inject: ['dragsort'],
  data() {
    return {
      isDraggable: true,
      isDraging: false,
      cssTransition: '',
      cssTransform: '',
    }
  },
  computed: {
    fwStyle() {
      const basis = `${100 / this.dragsort.colum}%`
      return {
        maxWidth: basis,
        flex: `0 0 ${basis}`,
        transition: this.cssTransition,
        transform: this.cssTransform,
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
      this.dragsort.dragId = this._id
      this.dragsort.dragIndex = this.index
    },

    handleDragend() {
      this.isDraging = false
      this.dragsort.dragEl = null
      this.dragsort.targetEl = null
      // 抛出真实数据
      this.dragsort._setRealSortData()
    },
    _animate(prevRect, targetEl) {
      var ms = this.dragsort.animation || 300
      if (ms) {
        const currentRect = targetEl.getBoundingClientRect()
        targetEl.__vue__.cssTransition = 'none'
        targetEl.__vue__.cssTransform = `translate3d(${prevRect.left - currentRect.left}px,${prevRect.top - currentRect.top}px,0)`

        setTimeout(() => {
          targetEl.__vue__.cssTransition = `all ${ms}ms`
          targetEl.__vue__.cssTransform = 'translate3d(0,0,0)'
        })
        clearTimeout(targetEl.animated)
        targetEl.animated = setTimeout(function () {
          targetEl.__vue__.cssTransition = ''
          targetEl.__vue__.cssTransform = ''
          targetEl.animated = false
        }, ms)
      }
    },
    handleDragover(e) {
      const dragEl = this.dragsort.dragEl
      if (dragEl.animated) {
        return
      }
      const targetEl = this.$refs.itemRef
      const dragId = this.dragsort.dragId
      const targeId = this._id
      const dragIndex = this.dragsort.dragIndex
      const targeIndex = this.index
      const dragRect = dragEl.getBoundingClientRect()
      const targetRect = targetEl.getBoundingClientRect()
      if (dragId !== targeId) {
        // 排序元素
        let ghostSortData = JSON.parse(JSON.stringify(this.dragsort.ghostSortData))
        this.dragsort._setGhostSortData(sortArr(ghostSortData, dragId, targeId))
        // 元素动画
        this.$nextTick(() => {
          this._animate(dragRect, dragEl)
          this._animate(targetRect, targetEl)
        }, 1000)
      }
    },
  },
}
</script>
