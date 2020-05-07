<template>
<div class="mainApp">
  <Dashboard />
</div>
</template>

<script>
import axios from 'axios'
import Dashboard from 'components/dashboard/Dashboard'

export default {
  components: {
    Dashboard
  }
}

</script>
<style lang="stylus" rel="stylesheet/stylus">
@import url('./assets/css/reset.css');
.mainApp
  position absolute
  height 100%
  width 100%
</style>