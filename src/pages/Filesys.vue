<template>
  <div class="">
    <button @click="uploader = true">add file</button>
    <el-uploader :open.sync="uploader"></el-uploader>
  </div>
</template>

<script>
import Uploader from '@/components/Uploader'

export default {
  name: 'Filesys',
  components: {
    'el-uploader': Uploader
  },
  data () {
    return {
      uploader: false
    }
  },
  methods: {
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>

</style>
