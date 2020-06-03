// 伪代码
<template>
  <custom-iframe :styles="styles">
    <vue-component />
  </custom-iframe>
</template>

----
<script>
import iframeStyles from '!raw-loader!sass-loader!./iframeStyles.css'

export default {
  data() {
    return {
       styles: iframeStyles
    }
  }
}
</script>
