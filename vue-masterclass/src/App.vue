<template>
<div>

  <the-navbar/>
  <div class="container">
    <router-view v-show="showPage" @ready="onPageReady" :key="$route.path"/>
    <AppSpinner v-show="!showPage" />
  </div>
</div>

</template>

<script>
import TheNavbar from '@/components/TheNavbar'
import { mapActions } from 'vuex'
import NProgress from 'nprogress'
export default {
  name: 'App',
  components: { TheNavbar },
  data () {
    return {
      showPage: false
    }
  },
  methods: {
    ...mapActions(['fetchAuthUser']),
    onPageReady () {
      this.showPage = true
      NProgress.done()
    }
  },
  screated () {
    this.fetchAuthUser()
    NProgress.configure({
      speed: 200,
      showSpinner: false
    })
    this.$router.beforeEach(() => {
      this.showPage = false
      NProgress.start()
    })
  }
}
</script>

<style>
@import "assets/style.css";
@import "~nprogress/nprogress.css";
/* @import "~bootstrap/css/bootstrap.css" gibi, index html de yazmamiz burda yazmamizin nedeni webpack sikistirsin css file ni diye*/  /* if we want to import from node_module we we can use ~ (shift+`) symbol */
#nprogress .bar{
  background: #57AD8D !important;
}
</style>
