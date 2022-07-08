<template>
  <div id="app">
    <Dragsort>
      <DragsortItem>asdasdasdasd</DragsortItem>
      <DragsortItem>asdasdasd</DragsortItem>
    </Dragsort>
  </div>
</template>
<script>
import DragsortItem from './components/DragsortItem.vue'
import Dragsort from './components/Dragsort.vue'
export default {
  components: { Dragsort, DragsortItem },
}
</script>
