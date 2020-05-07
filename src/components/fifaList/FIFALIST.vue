<template>
  <div class="container">
    <Card class="fifalist">
      <div slot="header">
        <span class="title">the best player </span>
      </div>
      <div class="column">
        <span>No</span>
        <span>Name</span>
        <span>Age</span>
        <span>Overall</span>
      </div>
      <div class="content">
        <div class="listitem column" v-for="i in activeData">
          <span>{{i.order}}</span>
          <span>{{i.Name}}</span>
          <span>{{i.Age}}</span>
          <span>{{i.Overall}}</span>
        </div>
      </div>
    </Card>
    <Card class="fifalist">
      <div slot="header">
        <span class="title">the best team </span>
      </div>
      <div class="column">
        <span>Position</span>
        <span>Name</span>
        <span>Age</span>
        <span>Overall</span>
        <span>Potential</span>
      </div>
      <div class="">
        <div class="listitem column" v-for="i in teamlength">
          <span>{{team.Position[i]}}</span>
           <span>{{team.Player[i]}}</span> 
          <span>{{team.Age[i]}}</span>
          <span>{{team.Overall[i]}}</span>
          <span>{{team.Potential[i]}}</span> 
        </div>
        <div style="margin-top: 15px">
          the average overall {{team.average}}
        </div>
      </div>
    </Card> 
  </div>
</template>

<script>
  import {Card} from 'element-ui';
  import axios from 'axios'

  export default {
    data() {
      return {
        list: {},
        listlength: 0,
        activeData: [],
        activeIndex: -1,
        team: {},
        teamlength: 0
      }
    },
    methods: {
      changeActive() {
        this.activeIndex++;
        this.activeData = [];
        if (this.activeIndex === 10) {
          this.activeIndex = 0;
        }
        let num = this.activeIndex;
        let count = this.listlength;
        let keys = Object.keys(this.list.Age);
        let min = num * 10;
        let max = (num + 1) * 10;
        for (let i = 0; i < count; i++) {
          if (i >= min && i < max) {
            let item = {};
            let key = keys[i];
            item.order = i + 1;
            item.Name = this.list.Name[key];
            item.Age = this.list.Age[key];
            item.Overall = this.list.Overall[key];
            this.activeData.push(item);
          }
        }
      },
      changeStart() {
        window.setInterval(() => {
          this.changeActive();
        }, 4000)
      }
    },
    components: {
      Card
    },
    created() {
      axios.get('/data/data.json').then(res => {
        this.list = res.data.obverall_sort;
        this.team = res.data.b;
        this.teamlength = Object.keys(this.team.Age).length;
        this.listlength = Object.keys(this.list.Age).length;
        this.changeActive();
        this.changeStart();
      });
    }
  }
</script>

<style lang="stylus" scoped>
  .container
    display flex
    flex-direction column
    justify-content flex-end
  .fifalist 
    background-color transparent
    border none
    color #fff
    height 45%
    overflow hidden
    margin-bottom 30px
    .content
      height 30vh
      overflow hidden
    .title
      font-size 24px
      font-weight 600
    .listitem
      line-height 3vh
  .el-card__body
    padding 10px 0
  .column
    display flex
    justify-content space-between
</style>