<template lang="html">
  <div class="dashboard">
    <div class="title">
      FIFA 2019 dataset visualization
    </div>
    <div class="flex-container column">
        <div class="item one" @click="clickChart('1')" style="transform: translate(-22.4%,-33.5%) scale(0.33)">
          <FIFAAGE></FIFAAGE>
        </div>
        <div class="item two" @click="clickChart('2')" style="transform: translate(-22.4%,0.5%) scale(0.33)">
          <FIFAOVERALL></FIFAOVERALL>
        </div>
        <div class="item three" @click="clickChart('3')" style="transform: translate(-22.4%,34.5%) scale(0.33)">
          <FIFAPOSITION></FIFAPOSITION>
        </div>
        <div class="item four active" @click="clickChart('4')" style="transform: translate(43.7%, 0) scale(1)">
          <FIFAWORLD></FIFAWORLD>
        </div>
    </div>
    <div class="datalist">
      <FIFALIST></FIFALIST>
    </div>
  </div>
</template>

<script>
import FIFAWORLD from 'components/fifaWorld/FIFAWORLD'
import FIFAAGE from 'components/fifaAge/FIFAAGE'
import FIFAOVERALL from 'components/fifaOverall/FIFAOVERALL'
import FIFAPOSITION from 'components/fifaPosition/FIFAPOSITION'
import FIFALIST from 'components/fifaList/FIFALIST'

export default {
  data() {
    return {
      items: []
    }
  },
  mounted() {
    this.init()
  },
  methods: {
    init() {
      this.items = document.querySelectorAll('.flex-container .item')
      for (let i = 0; i < this.items.length; i++) {
        this.items[i].dataset.order = i + 1;
      }
    },
    clickChart(clickIndex) {
      let activeItem = document.querySelector('.flex-container .active')
      let activeIndex = activeItem.dataset.order
      let clickItem = this.items[clickIndex - 1]
      if (activeIndex === clickIndex) {
        return
      }
      activeItem.classList.remove('active')
      clickItem.classList.add('active')
      this.setStyle(clickItem, activeItem)
    },
    setStyle(el1, el2) {
      let transform1 = el1.style.transform
      let transform2 = el2.style.transform
      el1.style.transform = transform2
      el2.style.transform = transform1
    }
  },
  components: {
    FIFAWORLD,
    FIFAAGE,
    FIFAOVERALL,
    FIFAPOSITION,
    FIFALIST
  }
}

</script>

<style lang="stylus" scoped>
*
  box-sizing: border-box;
.fifaworld,.fifaage,.fifaoverall,.fifaposition
  height 100%!important
  width 100%!important
  background none!important
.item
  padding: 0px;
  margin: 0px;
  margin-left: -100px;
  width: 55%;
  height: 100%;
  position absolute
  transform scale(0.33)
  text-align: center;
  transition:all 0.8s;
  background rgba(32, 32, 35, 0.5)
.dashboard
  position relative
  width 100%
  height 100%
  margin:0px;
  padding:0px;
  background url('../../assets/bg.jpg');
  background-size 100% 100%
.flex-container.column
  position relative
  height: 88%;
  width: 100%;
  overflow: hidden;
  box-sizing: content-box;
.active
  height 100%
  width: 60%;
  margin-left: -130px;
  line-height: 300px;
.title
  font-size 40px
  text-align center
  color #ffffff
  padding 20px 0
.datalist
  position absolute
  left 80%
  right 15px
  top 80px
</style>
