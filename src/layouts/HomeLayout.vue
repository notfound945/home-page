<template>
  <q-layout view="hHh lpR fFf">
    <q-header class="header-height bg-primary text-white">
      <q-toolbar>
        <q-toolbar-title>
        </q-toolbar-title>
        <q-btn v-if="!right" dense flat round icon="menu" color="primary" @click="right = !right"/>
        <q-btn v-else dense flat round icon="clear" color="primary" @click="right = !right"/>
      </q-toolbar>
    </q-header>

    <q-drawer show-if-above v-model="right" side="right" :width=380 elevated>
      <CollectionLinks></CollectionLinks>
    </q-drawer>

    <q-page-container>
      <router-view/>
    </q-page-container>

    <q-footer class="footer-trans purple-6 items-center text-white">
        <!--    底部版权信息-->
      <CopyrightBar></CopyrightBar>
    </q-footer>

  </q-layout>
</template>

<script>
import CollectionLinks from 'components/CollectionLinks'
import CopyrightBar from 'components/CopyrightBar'
export default {
  name: 'HomeLayout',
  components: { CopyrightBar, CollectionLinks },
  data () {
    return {
      right: false
    }
  }
}
</script>
<style lang="sass">
/*取消顶部 header 高度*/
.header-height
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
