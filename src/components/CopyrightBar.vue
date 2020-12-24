<template>
  <!--    底部版权信息-->
  <div class="q-ma-xs text-weight-light text-center">

    <a href="/#/home">
      <strong v-if="relativeUrl === '/#/home'"> {{ $t('home') }} </strong>
      <slot v-else> {{ $t('home') }} </slot>
    </a>
    |
    <q-icon name="copyright"/>
    <a href="https://notfound945.cn/oneindex" >
      {{ nowYear }} notfound945</a>
    |
    <a href="https://beian.miit.gov.cn/">
      湘ICP备19005461号</a>
    |
    <a href="/#/">
      <strong v-if="relativeUrl === '/#/'"> {{ $t('search') }} </strong>
      <slot v-else> {{ $t('search') }} </slot>
    </a>
  </div>
</template>

<script>
import { date } from 'quasar'
export default {
  name: 'CopyrightBar',
  data () {
    return {
      nowYear: '2021',
      relativeUrl: ''
    }
  },
  mounted () {
    const now = Date.now()
    this.nowYear = date.formatDate(now, 'YYYY')
    const url = document.location.toString()
    const arrUrl = url.split('//')
    const start = arrUrl[1].indexOf('/')
    // stop省略，截取从start开始到结尾的所有字符
    let relUrl = arrUrl[1].substring(start)
    if (relUrl.indexOf('?') !== -1) {
      relUrl = relUrl.split('?')[0]
    }
    this.relativeUrl = relUrl
  }
}
</script>

<style lang="sass" scoped>
/*a 标签去默认样式*/
a
  color: white
  text-decoration: none
</style>
