<template>
  <div id="app">
    {{ testData }}
    <Dragsort :colum="3" :sort-data="testData" @sort="handleSort">
      <template slot-scope="{ item }"> {{ item }} </template>
    </Dragsort>
  </div>
</template>
<script>
import Dragsort from './components/Dragsort.vue'
import '@/assets/dragsort.scss'
export default {
  components: { Dragsort },
  data() {
    return {
      testData: [
        { label: '1', value: 1 },
        { label: '2', value: 2 },
        { label: '3', value: 3 },
        { label: '4', value: 4 },
        { label: '5', value: 5 },
        { label: '6', value: 6 },
      ],
    }
  },
  methods: {
    handleSort(data) {
      console.log(
        '🚀 ~ data',
        data.map((item) => item.value)
      )
    },
  },
}
</script>

<style scoped>
.custm {
  height: 100px;
}
</style>
