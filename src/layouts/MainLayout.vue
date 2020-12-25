<template>
  <q-layout view="hHh lpR fFf"
            :style="{backgroundImage: bingImage.localUrl, backgroundPosition: 'center', backgroundSize: 'cover'}">
    <q-header elevated class="header bg-primary text-white">
      <q-toolbar>
        <q-toolbar-title></q-toolbar-title>
        <q-btn v-if="!right" dense flat round icon="menu" @click="right = !right"/>
        <q-btn v-else dense flat round icon="cancel" color="primary" @click="right = !right"/>
      </q-toolbar>
    </q-header>
    <q-page-container style="padding-bottom: 0">
      <div class="row justify-around items-center">
        <div class="fixed-bottom-left">
          <h1 class="now-time text-white">{{ nowTime }}</h1>
          <div class="image-description q-pa-md q-mb-xl text-subtitle1">
            <div>
              {{ bingImage.copyright }}
            </div>
            <div class="q-mt-md items-center justify-between row">
              <div class="text-grey text-subtitle2">
                喜欢这张壁纸吗?
              </div>
              <q-btn flat style="color: #808080;" round icon="save_alt" @click="downloadBingImage"/>
            </div>
          </div>
        </div>
      </div>
    </q-page-container>
    <q-drawer v-model="right" side="right" :width="400" bordered>
      <RightNavView></RightNavView>
    </q-drawer>
    <q-footer class="footer-trans bg-grey-8 text-white">
      <CopyrightBar></CopyrightBar>
    </q-footer>
  </q-layout>
</template>

<script>
import { date } from 'quasar'
import RightNavView from 'components/RightNavView'
import CopyrightBar from 'components/CopyrightBar'
import { getRequest } from 'src/axios'

export default {
  name: 'MainLayout',
  components: {
    CopyrightBar,
    RightNavView
  },
  data () {
    return {
      bingImage: {
        url: '',
        copyright: '',
        copyrightLink: '',
        blobLink: '',
        localUrl: ''
      },
      // 当前时间
      nowTime: null,
      // 右侧栏
      right: false
    }
  },
  async mounted () {
    // 获取Bing每日一图
    await getRequest('/getBing/HPImageArchive.aspx?format=js&idx=0&n=1&mkt=zh-CN')
      .then(res => {
        const image = res.data.images[0]
        this.bingImage.url = 'https://cn.bing.com/' + image.url
        this.bingImage.copyright = image.copyright
        this.bingImage.copyrightLink = image.copyrightlink
        this.getBingImageBlob(this.bingImage.url)
      })
    // 实时时间
    this.timer = setInterval(() => {
      const now = Date.now()
      this.nowTime = date.formatDate(now, 'HH:mm:ss')
    }, 1000)
  },
  methods: {
    // 获得本地 blob 链接地址
    getBingImageBlob (url) {
      const image = new Image()
      image.setAttribute('crossOrigin', 'anonymous')
      image.src = url
      image.onload = () => {
        const canvas = document.createElement('canvas')
        canvas.width = image.width
        canvas.height = image.height
        const ctx = canvas.getContext('2d')
        ctx.drawImage(image, 0, 0, image.width, image.height)
        canvas.toBlob((blob) => {
          const url = URL.createObjectURL(blob)
          this.bingImage.blobLink = url
          this.bingImage.localUrl = 'url(' + url + ')'
          // 用完释放URL对象
          // URL.revokeObjectURL(url)
        })
      }
    },
    // 下载图片
    downloadBingImage () {
      const eleLink = document.createElement('a')
      eleLink.download = 'th'
      eleLink.href = this.bingImage.blobLink
      eleLink.click()
      eleLink.remove()
    }

  },
  // 销毁定时器
  beforeDestroy () {
    if (this.timer) {
      clearInterval(this.timer)
    }
  }
}
</script>
<style lang="sass" scoped>
/*取消顶部 header 高度*/
.header
  height: 0
/*底部透明*/
.footer-trans
  height: 30px
  opacity: 0.7
  -moz-opacity: 0.7

/*图片描述框样式*/
.image-description
  max-height: 300px
  max-width: 60%
  background: rgba(0, 0, 0, 0.5)
  /*opacity: 0.4*/
  /*-moz-opacity: 0.4*/
  color: white
</style>
