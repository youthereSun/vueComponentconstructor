<template>
  <div >
    <div id="myCharts" ref="myCharts"></div>
  </div>
</template>
<script>

  import 'echarts/theme/helianthus'
  import echarts from 'echarts'

  export default {
    name: 'x-chart',

    mounted() {
      const myCharts = echarts.init(this.$refs.myCharts, "helianthus")
      myCharts.setOption(this.config)
    },
    props: {
      config: Object
    }
  }
</script>

<style scoped>

  #myCharts {
    width: 800px;
    height: 500px;
  }

</style>
