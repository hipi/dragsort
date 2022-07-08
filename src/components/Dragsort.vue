<template>
  <div class="fc-dragsort" @dragover="handleDragover">
    <DragsortItem v-for="(item, $index) in sortData" :key="$index" :item="item">
      <slot :$index="$index" :item="item" />
    </DragsortItem>
  </div>
</template>

<script>
import DragsortItem from './DragsortItem.vue'
export default {
  name: 'FcDragsort',
  components: { DragsortItem },
  provide() {
    return {
      dragsort: this,
    }
  },
  props: {
    colum: {
      type: Number,
      default: 2,
    },
    sortData: {
      type: Array,
      default: () => [],
    },
  },
  watch: {
    dragEl(val) {
      console.log('dragEl:', val?.innerHTML)
    },
    targetEl(val) {
      console.log('targetEl:', val?.innerHTML)
    },
  },
  data() {
    return {
      dragEl: null,
      targetEl: null,
      dragKey: null,
    }
  },
  methods: {
    handleDragover(e) {
      e.preventDefault()
    },
    _setNewSortData(sortData) {
      this.$emit('onSort', sortData)
      this.$emit('update:sortData', sortData)
    },
  },
}
</script>
