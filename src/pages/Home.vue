<template>
  <q-page class="flex flex-center" style="min-width: 50%">
    <div class="column">
      <div class="q-ma-md column flex-center" style="min-width: 300px">
        <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"
             xmlns:svgjs="http://svgjs.com/svgjs" version="1.1" width="64" height="64" x="0" y="0"
             viewBox="0 0 428.235 428.235" style="enable-background:new 0 0 512 512" xml:space="preserve" class=""><g transform="matrix(0.82,0,0,0.82,38.54115692138657,38.5411569213866)"><path xmlns="http://www.w3.org/2000/svg" d="m347.941 267.647h-26.765v-107.059h26.765c44.277 0 80.294-36.017 80.294-80.294s-36.017-80.294-80.294-80.294-80.294 36.017-80.294 80.294v26.765h-107.059v-26.765c0-44.277-36.017-80.294-80.294-80.294s-80.294 36.017-80.294 80.294 36.017 80.294 80.294 80.294h26.765v107.059h-26.765c-44.277 0-80.294 36.017-80.294 80.294s36.017 80.294 80.294 80.294 80.294-36.017 80.294-80.294v-26.765h107.059v26.765c0 44.277 36.017 80.294 80.294 80.294s80.294-36.017 80.294-80.294-36.017-80.294-80.294-80.294zm-26.765-187.353c0-14.755 12.01-26.765 26.765-26.765s26.765 12.01 26.765 26.765-12.01 26.765-26.765 26.765h-26.765zm-267.647 0c0-14.755 12.01-26.765 26.765-26.765s26.765 12.01 26.765 26.765v26.765h-26.765c-14.754 0-26.765-12.01-26.765-26.765zm53.53 267.647c0 14.755-12.01 26.765-26.765 26.765s-26.765-12.01-26.765-26.765 12.01-26.765 26.765-26.765h26.765zm53.529-187.353h107.059v107.059h-107.059zm187.353 214.118c-14.755 0-26.765-12.01-26.765-26.765v-26.765h26.765c14.755 0 26.765 12.01 26.765 26.765s-12.01 26.765-26.765 26.765z" fill="#474af9" data-original="#000000" style="" class=""/></g></svg>
      </div>
      <div class="q-ma-md column">
        <q-form @submit="submitWithEnterKey" class="q-gutter-md">
          <q-input bottom-slots
                   maxlength="20"
                   clearable
                   autofocus
                   v-model="searchText"
                   style="font-size: 18px"
                   :label="$t('startOnHere')"
                   :dense="dense">
            <template v-slot:prepend>
              <q-btn flat round
                     @click="changeSearchEngine">
                <svg width="28px" height="28px" class="icon" aria-hidden="true">
                  <use :href="searchEngineIcons[searchEngines[searchEngineFlag]]"></use>
                </svg>
              </q-btn>
            </template>
          </q-input>
        </q-form>
      </div>
      <div class="q-gutter-md flex-center text-center">
        <q-btn flat no-caps @click="doSearch(0)" style="color: #474af9" :label="$t('googleSearch')"/>
        <q-btn flat no-caps @click="doSearch(1)" style="color: #474af9" :label="$t('bingSearch')"/>
      </div>
      <div class="q-gutter-md q-ma-md flex-center text-center">
        <q-btn-group flat>
          <q-btn flat color="pink-6" @click="switchLanguage(0)" label="中 文" />
          <q-btn flat no-caps color="pink-6" @click="switchLanguage(1)" label="English" />
        </q-btn-group>
      </div>
      <div class="desktop-only q-mt-md flex-center text-center">
        <q-img
          :src="qrcode"
          style="height: 100px; max-width: 100px"
        >
          <template v-slot:loading>
            <div class="text-subtitle1 text-white">
              Loading...
            </div>
          </template>
        </q-img>
        <div class="text-subtitle text-teal-5">
          {{ $t('openWithPhone') }}
        </div>
      </div>
    </div>
  </q-page>
</template>

<script>
import QRCode from 'qrcode'
export default {
  name: 'Home',
  data () {
    return {
      dense: false,
      searchText: '',
      number: '',
      qrcode: '',
      lang: this.$i18n.locale,
      searchEngineIcons: {
        google: '#icongoogle',
        bing: '#iconbing',
        baidu: '#iconbaidu'
      },
      searchEngineFlag: 0,
      searchEngines: ['google', 'bing', 'baidu'],
      searchEngineUrls: {
        google: 'google.com.hk/search?hl=zh&q=',
        bing: 'cn.bing.com/search?q=',
        baidu: 'baidu.com/s?ie=utf-8&word='
      },
      // 建议链接
      suggestionUrls: [
        {
          name: 'OneIndex',
          icon: 'cloud_circle',
          url: 'https://notfound945.cn/oneindex/#/'
        }, {
          name: '教务系统（校外）',
          icon: 'cast_for_education',
          url: 'https://webvpn.jsu.edu.cn'
        }
      ]
    }
  },
  async mounted () {
    // 生成当前地址二维码
    const currentUrl = window.location.href
    // With async/await
    const generateQR = async text => {
      try {
        this.qrcode = await QRCode.toDataURL(text,
          {
            errorCorrectionLevel: 'H',
            color: {
              dark: '#474af9'
            }
          })
      } catch (err) {
        console.error('二维码生成错误 ', err)
      }
    }
    await generateQR(currentUrl)
  },
  methods: {
    changeSearchEngine () {
      if (this.searchEngineFlag >= 2) {
        this.searchEngineFlag = 0
      } else {
        this.searchEngineFlag = this.searchEngineFlag + 1
      }
    },
    submitWithEnterKey () {
      window.location.href = 'https://' + this.searchEngineUrls[this.searchEngines[this.searchEngineFlag]] + this.searchText
    },
    doSearch (type) {
      window.location.href = 'https://' + this.searchEngineUrls[this.searchEngines[type]] + this.searchText
    },
    switchLanguage (index) {
      if (index === 0) {
        this.lang = 'zh-hans'
      } else {
        this.lang = 'en-us'
      }
    }
  },
  watch: {
    lang (lang) {
      this.$i18n.locale = lang
    }
  }
}
</script>

<style scoped>

</style>
