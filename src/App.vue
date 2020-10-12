<template>
  <div id="app">
    <button @click="handleIncrement">更改vuex状态</button>
    下面是iframe
    <custom-iframe
      :styles="iframeStyles"
      style="border: 1px solid;min-height: 400px;"
    >
      ifrmae区域
      <img src="./assets/logo.png" alt="vue logo" style="display: block;" />
      <custom-title title="自定义组件" />
      vuex-count：{{ count }}
    </custom-iframe>
  </div>
</template>

<script>
import { mapState } from 'vuex'

import CustomIframe from './components/CustomIframe.vue'
import CustomTitle from './components/CustomTitle.vue'
import iframeStyles from '!raw-loader!sass-loader!./iframeStyles.scss'

export default {
  name: 'App',
  components: {
    CustomIframe,
    CustomTitle
  },
  data() {
    return {
      iframeStyles
    }
  },
  computed: {
    ...mapState({
      count: (state) => state.count
    })
  },
  methods: {
    handleIncrement() {
      this.$store.commit('increment')
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
