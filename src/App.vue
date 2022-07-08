<template>
  <div id="app">
    {{ a }}
    <Dragsort :colum="3" :sort-data.sync="a">
      <template slot-scope="scope"> {{ scope }} </template>
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
      a: [1, 22, 333, 4444, 55555, 666666],
    }
  },
}
</script>

<style scoped>
.custm {
  height: 100px;
}
</style>
