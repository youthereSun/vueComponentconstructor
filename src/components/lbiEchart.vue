<template>
  <div id="lbi_echart">
    <div v-show="!showSub">
      <div  id="myCharts" ref="myCharts"></div>
    </div>
    <div v-show="showSub">
      <button @click="showSub=false">返回</button>
      <div id="mySubCharts" ref="mySubCharts"></div>
    </div>

  </div>
</template>
<script>

  import 'echarts/theme/bee-inspired'
  import 'echarts/theme/blue'
  import 'echarts/theme/caravan'
  import 'echarts/theme/carp'
  import 'echarts/theme/forest'
  import 'echarts/theme/dark-blue'
  import 'echarts/theme/jazz'
  import 'echarts/theme/fruit'
  import 'echarts/theme/london'
  import 'echarts/theme/red'
  import 'echarts/theme/vintage'
  import 'echarts/theme/tech-blue'
  import 'echarts/theme/sakura'
  import 'echarts/theme/shine'
  import 'echarts/theme/helianthus'
  import 'echarts/theme/roma'
  import 'echarts/theme/royal'
  import 'echarts/theme/macarons'
  import 'echarts/theme/macarons2'
  import 'echarts/theme/inspired'
  import 'echarts/theme/infographic'
  import 'echarts/theme/green'
  import 'echarts/theme/dark-mushroom'
  import echarts from 'echarts'

  export default {
    name: 'lw-lbi-chart',
    data () {
      return {
        config: {},
        showSub: false
      }
    },
    created () {
    },
    mounted () {

      var that = this
      that.getConfigurationByCode().then(res => {
        that.config = JSON.parse(res.data.config)
        const myCharts = echarts.init(that.$refs.myCharts, that.config.otherSetting.chartTheme)
        myCharts.setOption(that.config)
        myCharts.on('click', function (param) {
          const { name, seriesName, seriesIndex } = param
          const params = new Object()
          params['parent_x'] = name
          params['parent_series'] = seriesName
          const code = params['code'] = that.config.series[seriesIndex].subSeriesConfigurationCode
          if (code) {
            that.getSubChart(params)
          }
        })
      }).catch(error => {

      })

    },

    methods: {
      /**
       * 首次进入时通过code获取config
       * @returns {Promise<unknown>}
       */
      getConfigurationByCode () {
        const params = {
          code: this.attributes.code
        }
        return new Promise((resolve, reject) => {
          this.$http.post('/handler/biHandler/echartGet', params).then(function (res) {
            resolve(res)
            // eslint-disable-next-line handle-callback-err
          }, function (error) {
            reject(error)
          })
        })
      },
      getSubConfigurationByCode (params) {
        return new Promise((resolve, reject) => {
          this.$http.post('/handler/biHandler/echartGet', params).then(function (res) {
            resolve(res)
            // eslint-disable-next-line handle-callback-err
          }, function (error) {
            reject(error)
          })
        })
      },
      /**
       * 点击获取子图表数据
       * @param params
       */
      getSubChart (params) {
        var that = this
        that.getSubConfigurationByCode(params).then(res => {
          that.showSub = true
          const subConfig = JSON.parse(res.data.config)
          const mySubChart = echarts.init(that.$refs.mySubCharts, subConfig.otherSetting.chartTheme)
          mySubChart.setOption(subConfig)
        }).catch(error => {

        })
      }
    },
    props: {
      attributes: Object,
      children: Array
    }
  }
</script>

<style scoped>

  #myCharts,#mySubCharts {
    width: 800px;
    height: 500px;
  }

</style>
