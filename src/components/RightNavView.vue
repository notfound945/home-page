<template>
  <div class="col">
    <h5 class="text-center">常用导航</h5>
    <div class="flex q-gutter-md justify-around">
      <q-card class="nav-card" v-for="(item, index) in nav" :key="index">
        <a :href="item.href" target="_blank">
          <q-img v-if="item.navImg !== null"
                 :src="item.navImg"
                 :ratio="1"
                 spinner-color="red"
                 :style="item.style"
                 class="rounded-borders"
                 contain
          >
            <div class="absolute-bottom text-weight-medium text-center" style="height: 40px">
              {{ item.navTitle }}
            </div>
            <template v-slot:loading>
              <div class="text-subtitle1 text-pink">
                Loading...
              </div>
            </template>
            <template v-slot:error>
              <div class="absolute-full flex flex-center bg-negative text-teal-5">
                Load image error
              </div>
            </template>
          </q-img>
        </a>
      </q-card>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LeftNavView',
  mounted () {
  },
  data () {
    return {
      // 导航数据
      nav: [
        {
          navImg: 'http://ww1.sinaimg.cn/large/007eYQjmgy1gly1ok1v8xj30e80e8dfx.jpg',
          navTitle: 'OneIndex',
          href: 'https://www.notfound945.cn/oneindex',
          style: 'height: 100px'
        }, {
          navImg: 'http://ww1.sinaimg.cn/large/007eYQjmgy1gly2qere59j30p00o6gza.jpg',
          navTitle: '教务系统（校外）',
          href: 'https://webvpn.jsu.edu.cn/',
          style: 'height: 100px'
        }
      ]
    }
  }
}
</script>

<style lang="sass" scoped>
.nav-card
  width: 170px
  max-width: 190px
</style>
