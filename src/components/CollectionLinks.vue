<template>
  <div class="flex-center text-center">
    <div class="flex-center">
      <q-img :src="bingImage.blobLink">
        <q-btn class="absolute-bottom-right q-ma-xs text-teal-5" round @click="downloadBingImage"
               icon="save_alt"></q-btn>
        <template v-slot:error>
          <div class="absolute-full flex flex-center bg-negative text-white">
            Cannot load image
          </div>
        </template>
      </q-img>
      <div class="q-ma-md text-subtitle2">
        <a :href="bingImage.copyrightLink" target="_blank"> {{ bingImage.copyright }} </a>
      </div>
      <div v-for="(list, index) in links" v-bind:key="index" class="q-pa-md justify-around">
        <div class="row justify-start q-ma-xs items-center">
          <svg width="20px" height="20px" class="icon" aria-hidden="true">
            <use :href="list.icon"></use>
          </svg>
          <div class="text-h5 q-ml-xs">
            {{ list.type }}
          </div>
        </div>
        <q-separator class="q-ma-xs"/>
        <div class="flex justify-around">
          <div v-for="(link, index) in list.content" v-bind:key="index" class="flex q-ma-xs justify-between">
            <q-btn class="text-center justify-between" @click="toPage(link.url)" flat style="color: #FF0080">
              <svg width="20px" height="20px" class="icon" aria-hidden="true">
                <use :href="link.icon"></use>
              </svg>
              <div class="q-ml-xs text-center">
                {{ link.name }}
              </div>
            </q-btn>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { getRequest } from 'src/axios'

export default {
  name: 'CollectionLinks',
  data () {
    return {
      bingImage: {
        url: '',
        copyright: '',
        copyrightLink: '',
        blobLink: ''
      },
      links: [{
        // TODO 视频类
        type: 'video',
        icon: '#icon-shipin',
        content: [{
          name: 'Youtube',
          url: '//www.youtube.com/',
          icon: '#icon-youtube',
          color: 'primary'
        }, {
          name: '腾讯视频',
          url: '//v.qq.com/',
          icon: '#icon-tengxunshipin',
          color: 'primary'
        }, {
          name: '优酷',
          url: '//www.youku.com/',
          icon: '#icon-youku',
          color: 'primary'
        }, {
          name: '爱奇艺',
          url: '//www.iqiyi.com/',
          icon: '#icon-aiqiyi',
          color: 'primary'
        }, {
          name: '字幕组',
          url: '//www.zmz2019.com/',
          icon: '#icon-zimu',
          color: 'primary'
        }, {
          name: 'ACFUN',
          url: '//www.acfun.cn/index.html',
          icon: '#icon-acfun',
          color: 'primary'
        }, {
          name: '哔哩哔哩',
          url: '//www.bilibili.com/',
          icon: '#icon-bili',
          color: 'primary'
        }]
      }, {
        // TODO 邮箱类
        type: 'mail',
        icon: '#icon-youxiang',
        content: [{
          name: 'Gmail',
          url: '//mail.google.com/mail/u/0/#inbox',
          icon: '#icon-gmail',
          color: 'primary'
        }, {
          name: 'Hotmail',
          url: '//outlook.live.com/mail/',
          icon: '#icon-windows',
          color: 'primary'
        }, {
          name: '网易邮箱',
          url: '//mail.163.com/',
          icon: '#icon-wangyi',
          color: 'primary'
        }, {
          name: '新浪邮箱',
          url: '//mail.sina.com.cn/',
          icon: '#icon-xinlangwang',
          color: 'primary'
        }, {
          name: 'QQ邮箱',
          url: '//mail.qq.com/',
          icon: '#icon-QQ',
          color: 'primary'
        }, {
          name: '阿里邮箱',
          url: '//qiye.aliyun.com/',
          icon: '#icon-yunyouxiang',
          color: 'primary'
        }]
      }, {
        // TODO 社交类
        type: 'social',
        icon: '#icon-msg',
        content: [{
          name: '微博',
          url: '//www.weibo.com',
          icon: '#icon-weibo',
          color: 'primary'
        }, {
          name: '知乎',
          url: '//www.zhihu.com/',
          icon: '#icon-zhihu',
          color: 'primary'
        }, {
          name: '豆瓣',
          url: '//www.douban.com/',
          icon: '#icon-douban',
          color: 'primary'
        }, {
          name: '简书',
          url: '//www.jianshu.com/',
          icon: '#icon-jianshu',
          color: 'primary'
        }, {
          name: 'V2EX',
          url: '//www.v2ex.com/',
          icon: '#icon-v2ex',
          color: 'primary'
        }, {
          name: 'Instagram',
          url: '//www.instagram.com',
          icon: '#icon-instagram',
          color: 'primary'
        }, {
          name: 'Twitter',
          url: '//www.twitter.com',
          icon: '#icon-twitter',
          color: 'primary'
        }, {
          name: 'Facebook',
          url: '//www.facebook.com',
          icon: '#icon-facebook',
          color: 'primary'
        }]
      }, {
        // TODO 购物类
        type: 'shopping',
        icon: '#icon-gouwuche',
        content: [{
          name: '淘宝网',
          url: '//www.taobao.com',
          icon: '#icon-taobao',
          color: 'primary'
        }, {
          name: '聚划算',
          url: '//dyartstyle.com/juhuasuan/',
          icon: '#icon-juhuasuan',
          color: 'primary'
        }, {
          name: '淘宝特卖',
          url: '//dyartstyle.com/temai/',
          icon: '#icon-temai',
          color: 'primary'
        }, {
          name: '拼多多',
          url: '//mobile.yangkeduo.com/',
          icon: '#icon-99',
          color: 'primary'
        }, {
          name: '京东',
          url: '//www.jd.com',
          icon: '#icon-jingdong',
          color: 'primary'
        }, {
          name: '苏宁易购',
          url: '//www.suning.com/',
          icon: '#icon-suning',
          color: 'primary'
        }, {
          name: '网易严选',
          url: '//you.163.com/',
          icon: '#icon-yanxuan',
          color: 'primary'
        }, {
          name: '亚马逊',
          url: '//www.amazon.cn/',
          icon: '#icon-amazon',
          color: 'primary'
        }, {
          name: '当当',
          url: '//www.dangdang.com/',
          icon: '#icon-dangdang',
          color: 'primary'
        }, {
          name: '吾爱淘',
          url: '//wat.dyartstyle.com/',
          icon: '#icon-wat',
          color: 'primary'
        }]
      }, {
        type: 'design',
        icon: '#icon-sheji',
        content: [{
          // TODO 设计类
          name: '设计导航',
          url: '//web.yyv.me/',
          icon: '#icon-daohang1',
          color: 'primary'
        }, {
          name: 'Pinterest',
          url: '//www.pinterest.com/',
          icon: '#icon-pinterest',
          color: 'primary'
        }, {
          name: 'Behance',
          url: '//www.behance.net/',
          icon: '#icon-behance',
          color: 'primary'
        }, {
          name: 'Dribbble',
          url: '//www.dribbble.com/',
          icon: '#icon-dribbble',
          color: 'primary'
        }, {
          name: '花瓣',
          url: '//huaban.com/',
          icon: '#icon-huaban',
          color: 'primary'
        }, {
          name: '站酷',
          url: '//www.zcool.com.cn/',
          icon: '#icon-zhanku',
          color: 'primary'
        }, {
          name: '阿里图标',
          url: '//www.iconfont.cn/',
          icon: '#icon-iconfont',
          color: 'primary'
        }, {
          name: 'IconFinder',
          url: '//www.iconfinder.com/',
          icon: '#icon-eye',
          color: 'primary'
        }, {
          name: '优设教程',
          url: '//uiiiuiii.com/',
          icon: '#icon-jiaocheng',
          color: 'primary'
        }]
      }, {
        // TODO 搜索引擎类
        type: 'search',
        icon: '#icon-sousuo',
        content: [{
          name: 'Google',
          url: '//www.google.com/',
          icon: '#icon-google',
          color: 'primary'
        }, {
          name: 'DuckGo',
          url: '//duckduckgo.com/',
          icon: '#icon-duck',
          color: 'primary'
        }, {
          name: 'Bing',
          url: '//www.bing.com/',
          icon: '#icon-bing',
          color: 'primary'
        }, {
          name: '百度',
          url: '//baidu.com/',
          icon: '#icon-icon_baidulogo',
          color: 'primary'
        }, {
          name: '雅虎',
          url: '//hk.yahoo.com/',
          icon: '#icon-yahoo',
          color: 'primary'
        }, {
          name: '搜狗',
          url: '//www.sogou.com/',
          icon: '#icon-sougou',
          color: 'primary'
        }, {
          name: 'NAVER',
          url: '//www.naver.com/',
          icon: '#icon-icon-test',
          color: 'primary'
        }, {
          name: '秘迹',
          url: '//mijisou.com/',
          icon: '#icon-miji',
          color: 'primary'
        }, {
          name: '多吉',
          url: '//www.dogedoge.com/',
          icon: '#icon-gougou',
          color: 'primary'
        }, {
          name: 'seeres',
          url: '//seeres.com/',
          icon: '#icon-sousuo',
          color: 'primary'
        }]
      }, {
        // TODO 工具类
        type: 'tools',
        icon: '#icon-ai-tool',
        content: [{
          name: 'Miku工具',
          url: '//tools.miku.ac/',
          icon: '#icon-ai-tool',
          color: 'primary'
        }, {
          name: '谷歌翻译',
          url: '//translate.google.cn/?hl=zh-CN',
          icon: '#icon-fanyi',
          color: 'primary'
        }, {
          name: '史莱姆',
          url: '//www.slimego.cn/',
          icon: '#icon-shilaimu',
          color: 'primary'
        }, {
          name: 'Feedly',
          url: '//feedly.com',
          icon: '#icon-feedly',
          color: 'primary'
        }, {
          name: '百度网盘',
          url: '//pan.baidu.com',
          icon: '#icon-baiduyun',
          color: 'primary'
        }, {
          name: 'MD编辑器',
          url: '//www.mdeditor.com/',
          icon: '#icon-md',
          color: 'primary'
        }, {
          name: '贝赛尔曲线',
          url: '//cubic-bezier.com',
          icon: '#icon-quxian',
          color: 'primary'
        }, {
          name: 'JS混淆器',
          url: '//javascriptobfuscator.com/Javascript-Obfuscator.aspx',
          icon: '#icon-jshunxiao',
          color: 'primary'
        }, {
          name: 'Ping.pe',
          url: '//ping.pe',
          icon: '#icon-wangluo1',
          color: 'primary'
        }, {
          name: '站长Ping',
          url: '//ping.chinaz.com/',
          icon: '#icon-pingup',
          color: 'primary'
        }, {
          name: 'APK下载',
          url: '//apkdl.in/',
          icon: '#icon-anzhuo',
          color: 'primary'
        }]
      }, {
        // TODO 开发类
        type: 'development',
        icon: '#icon-kongzhitai',
        content: [{
          name: 'W3school',
          url: '//www.w3school.com.cn/',
          icon: '#icon-h5',
          color: 'primary'
        }, {
          name: 'Github',
          url: '//github.com/',
          icon: '#icon-github',
          color: 'primary'
        }, {
          name: 'Codepen',
          url: '//codepen.io/',
          icon: '#icon-codepen',
          color: 'primary'
        }, {
          name: '吾爱破解',
          url: '//www.52pojie.cn/',
          icon: '#icon-theater-masks',
          color: 'primary'
        }, {
          name: 'SF思否',
          url: '//segmentfault.com/',
          icon: '#icon-msg',
          color: 'primary'
        }, {
          name: 'CdnJs',
          url: '//cdnjs.com/',
          icon: '#icon-cdnjs',
          color: 'primary'
        }, {
          name: 'Font A.',
          url: '//fontawesome.com/icons?d=gallery&m=free',
          icon: '#icon-font-awesome',
          color: 'primary'
        }, {
          name: 'MSDN下载',
          url: '//msdn.itellyou.cn/',
          icon: '#icon-windows',
          color: 'primary'
        }, {
          name: 'C. flare',
          url: '//dash.cloudflare.com/',
          icon: '#icon-cloudflare',
          color: 'primary'
        }, {
          name: 'Swiper',
          url: '//www.swiper.com.cn/',
          icon: '#icon-S',
          color: 'primary'
        }]
      }]
    }
  },
  async mounted () {
    // 请求 Bing 每日一图
    await getRequest('/getBing/HPImageArchive.aspx?format=js&idx=0&n=1&mkt=zh-CN')
      .then(res => {
        const image = res.data.images[0]
        this.bingImage.url = 'https://cn.bing.com/' + image.url
        this.bingImage.copyright = image.copyright
        this.bingImage.copyrightLink = image.copyrightlink
        this.getBingImageBlob(this.bingImage.url)
      })
    await getRequest('/getLinks').then(res => {
      if (res.data != null) {
        console.log('得到服务器响应数据，使用服务器响应数据')
        console.log(res.data)
        this.links = res.data
      } else {
        console.log('请求服务器出错了！使用默认预置数据')
      }
    })
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
          this.bingImage.blobLink = URL.createObjectURL(blob)
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
    },
    createSuperLabel (url, id) {
      const a = document.createElement('a')
      a.setAttribute('href', url)
      a.setAttribute('target', '_blank')
      a.setAttribute('id', id)
      // 防止反复添加
      if (!document.getElementById(id)) {
        document.body.appendChild(a)
      }
      a.click()
      a.remove()
    },
    toPage (url) {
      const id = 'new_a'
      this.createSuperLabel(url, id)
    }
  }
}
</script>

<style lang="sass" scoped>

/*a 标签去默认样式*/
a
  color: #f66262
  text-decoration: none
</style>
