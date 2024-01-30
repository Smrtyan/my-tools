<template>
  <div>
    <h2>My Tools</h2>

    <el-button
      type="success"
      size="mini"
      circle
      @click="go"
    >Go</el-button>
  </div>

</template>

<script>

import service from '@/utils/my-request'
export default {

  data() {
    return {
      chart: null
    }
  },
  mounted() {

  },
  methods: {
    go() {
      service.get('http://192.168.0.114:8000/api/hello/').then((data) => {
        console.log(data)
      })
    }

  }
}
</script>
