<template>
  <div class="fc-dragsort" @dragover="handleDragover">
    <DragsortItem v-for="(list, $index) in ghostSortData" :key="list._id" :_id="list._id" :item="list.value" :index="$index">
      <slot :$index="$index" :item="list.value" />
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
  data() {
    return {
      dragEl: null,
      targetEl: null,
      dragIndex: null,
      dragId: null,
      ghostSortData: [],
    }
  },
  watch: {
    sortData: {
      handler(data) {
        this.ghostSortData = data.map((list, index) => ({
          _id: index + JSON.stringify(list),
          value: list,
        }))
      },
      deep: true,
      immediate: true,
    },
  },
  methods: {
    handleDragover(e) {
      e.preventDefault()
    },
    _setGhostSortData(sortData) {
      this.ghostSortData = sortData
    },
    _setRealSortData() {
      const realSortData = this.ghostSortData.map((item) => item.value)
      this.$emit('sort', realSortData)
      this.$emit('update:sortData', realSortData)
    },
  },
}
</script>
