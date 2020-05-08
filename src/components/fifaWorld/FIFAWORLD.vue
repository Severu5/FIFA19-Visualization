<template>
  <div class="fifaworld">
    <div class="chart" id="fifaworld"></div>
  </div>
</template>

<script>
  import axios from 'axios'
  import echarts from 'echarts'
  import world from 'echarts/map/js/world.js'

  export default {
    name: "FIFAWORLD",
    data() {
      return {
        keys: [],
        data: {}
      }
    },
    created() {
      this.getWorld();
    },
    methods: {
      getWorld() {
        axios.get('./data/data.json').then(res => {
          this.data = res.data.world;
          this.keys = Object.keys(this.data);
        })
        axios.get('./data/World.json').then(res => {
          let namemap = res.data.namemap;
          let dataArr = res.data.dataArr;
          this.data = this.dataFormat(this.data, namemap);
          this.drawChart(namemap, this.data)
        })
      },
      drawChart(name, data) {
        let chart = echarts.init(document.getElementById('fifaworld'))
        window.addEventListener('resize', () => {
          chart.resize()
        })
        chart.setOption({
          title: {
            text: 'FIFA19 Player Nationality (top 100)',
            top: 20,
            left: 'center',
            textStyle: {
              fontSize: 24,
              fontWeight: 600,
              color: '#fff'
            },
            subtext: '左侧点击可放大'
          },
          tooltip: {
            trigger: 'item',
            formatter (val) {
              return val.data==undefined?undefined:val.data.name+': '+(val.data.value === undefined ? 0 : val.data.value);
            }
          },
          visualMap: {
            type: 'piecewise',
            show: true,
            min: 0,
            max: 1700,
            textStyle: {
              fontSize: 16,
              color: '#fff'
            },
            realtime: false,
            calculable: true,
            inRange: {
              color: ['lightskyblue', '#e6ac53', '#74e2ca', 'yellow', 'orangered', 'red']
            }
          },
          series: [
            {
              type: 'map',
              name: 'FIFA19 Player Nationality',
              mapType: 'world',
              roam: true,
              label: {
                show: false
              },
              itemStyle: {
                areaColor: '#7B68EE',
                borderWidth: 0.5,
                borderColor: '#000',
                borderType: 'solid'
              },
              emphasis: {
                label: {
                  show: true,
                  color: '#000'
                },
                itemStyle: {
                  areaColor: '#fee090'
                }
              },
              nameMap: name,
              data: data
            }
          ]
        })
        let index = -1
        setInterval(() => {
          chart.dispatchAction({
            type: 'hideTip',
            seriesIndex: 0,
            dataIndex: index
          })
          chart.dispatchAction({
            type: 'showTip',
            seriesIndex: 0,
            dataIndex: index + 1
          })
          chart.dispatchAction({
            type: 'downplay',
            seriesIndex: 0,
            dataIndex: index
          })
          chart.dispatchAction({
            type: 'highlight',
            seriesIndex: 0,
            dataIndex: index + 1
          })
          index++
          if (index > data.length - 1) {
            index = -1
          }
        }, 2000)
      },
      dataFormat(data, namemap) {
        let newArr = [];
        let keys = this.keys;
        for (let item of keys) {
          newArr.push({
            "name": namemap[item],
            "value": parseFloat(data[item])
          })
        }
        return newArr;
      }
    }
  }
</script>

<style lang="stylus" scoped>
.fifaworld
  .chart
    height 100%
    width 100%
    transition all 0.5s linear
</style>
