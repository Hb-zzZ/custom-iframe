<script>
import Vue from 'vue'

export default {
  props: {
    onMounted: {
      type: Function,
      default: function() {}
    },
    styles: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      iApp: null
    }
  },
  watch: {
    styles: {
      handler() {
        this.loadStyles()
      }
    }
  },
  render(h) {
    return h('iframe', {
      style: { height: '100%', width: '100%', border: '0' },
      on: { load: this.renderChildren }
    })
  },
  beforeUpdate() {
    this.iApp.children = Object.freeze(this.$slots.default)
  },
  methods: {
    loadStyles() {
      const head = this.$el.contentDocument.head

      let stylesString = this.styles

      if (!/(<style.*?<\/style>)/.test(stylesString)) {
        stylesString = `<style> ${stylesString} </style>`
      }
      head.innerHTML = stylesString
    },
    renderChildren() {
      const children = this.$slots.default
      const body = this.$el.contentDocument.body
      const el = document.createElement('div')
      body.appendChild(el)

      const iApp = new Vue({
        name: 'iApp',
        //freezing to prevent unnessessary Reactifiation of vNodes
        data: { children: Object.freeze(children) },
        render(h) {
          return h('div', this.children)
        }
      })

      iApp.$mount(el) // mount into iframe

      this.loadStyles()
      this.iApp = iApp
      this.onMounted()
    }
  }
}
</script>
