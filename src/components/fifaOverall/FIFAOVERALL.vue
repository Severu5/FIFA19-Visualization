<template>
  <div class="fifaoverall">
    <div class="chart" id="fifaoverall"></div>
  </div>
</template>

<script>
  import echarts from 'echarts'
  import axios from 'axios'

  export default {
    methods: {
      getMulCol() {
        axios.get('./data/data.json').then(res => {
          let data = res.data.overall;
          this.drawChart(data);
        })
      },
      drawChart(data) {
        let chart = echarts.init(document.getElementById('fifaoverall'))
        window.addEventListener('resize', () => {
          chart.resize()
        })
        chart.setOption({
          title: {
            show: true,
            text: 'FIFA19 Overall ',
            top: 20,
            left: 'center',
            textStyle: {
              fontSize: 24,
              fontWeight: 600,
              color: '#fff'
            },
            subtext: 'fifa 2019 球员整体数据展示'
          },
          tooltip: {
            trigger: 'axis',
            formatter (val) {
              return val[0].axisValue + ': ' + val[0].data + '个';
            }
          },
          calculable: true,
          xAxis: [{
            name: 'Overall',
            nameTextStyle: {
              color: 'rgba(255, 255, 255, 0.69)'
            },
            axisLabel: {
              textStyle: {
                color: '#fff'
              }
            },
            data: Object.keys(data)
          }],
          yAxis: [{
            nameLocation: 'end',
            nameGap: 20,
            nameRotate: 0,
            splitLine: {
              lineStyle: {
                color: ['rgba(230, 230, 230, 0.2)']
              }
            },
            axisLabel: {
              textStyle: {
                color: '#fff',
                fontSize: 14
              }
            },
            name: 'count',
            type: 'value',
            nameTextStyle: {
              color: 'rgba(255, 255, 255, 0.69)'
            }
          }],
          visualMap: {
            type: 'continuous',
            show: true,
            min: 0,
            max: 1200,
            left: 'right',
            top: 'top',
            textStyle: {
              fontSize: 14,
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
              type: 'bar',
              emphasis: {
                itemStyle: {
                  color: new echarts.graphic.LinearGradient(
                    0, 0, 0, 1,
                    [
                      {offset: 0, color: '#2378f7'},
                      {offset: 0.7, color: '#2378f7'},
                      {offset: 1, color: '#83bff6'}
                    ]
                  )
                }
              },
              data: Object.values(data)
            }
          ]
        });
      }
    },
    mounted() {
      this.getMulCol();
    }
  }

</script>

<style lang="stylus" scoped>
.fifaoverall
  .chart
    height 100%
    width 100%
    transition all 0.5s linear
</style>
