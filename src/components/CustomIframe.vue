<script>
import Vue from 'vue'
export default {
  props: {
    onMounted: {
      type: Function,
      default: function() {}
    },
    // 自定义样式
    styles: {
      type: String,
      default: ''
    },
    // 自定义css link
    links: {
      type: Array,
      default() {
        return []
      }
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
    },
    links: {
      handler() {
        this.loadLinks()
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
    loadLinks() {
      const { links } = this
      const doc = document
      const head = this.$el.contentDocument.head
      const frag = document.createDocumentFragment()
      const loadLinks = links.map((item) => {
        const link = doc.createElement('link')
        for (const key in item) {
          link.setAttribute(key, item[key])
        }
        return link
      })
      loadLinks.forEach((item) => frag.appendChild(item))
      head.appendChild(frag)
    },
    loadStyles() {
      const doc = document
      const head = this.$el.contentDocument.head
      const style = doc.createElement('style')

      if (style.styleSheet) {
        // IE
        style.styleSheet.cssText = this.styles
      } else {
        const cssText = doc.createTextNode(this.styles)
        style.appendChild(cssText)
      }
      head.appendChild(style)
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
      this.iApp = iApp
      this.afterRender()
    },
    afterRender() {
      this.loadLinks()
      this.loadStyles()
      this.onMounted()
    }
  }
}
</script>
