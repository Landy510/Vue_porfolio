<template>
  <div class="wrap">
    <div class="progress rounded-0">
      <div class="progress-content" :style="{width: `${progress}%`}"></div>
    </div>
    <div class="flex-grow-1">
      <router-view/>
    </div>
    <Footer/>
  </div>
</template>

<script>
import Footer from '@/components/Footer.vue'
export default {
  name: 'CustomerOrder',
  data () {
    return {

    }
  },
  computed: {
    progress () {
      if ((/\/customerOrder\/customer1/).test(this.$route.path)) {
        return 25
      } else if ((/\/customerOrder\/customer2/).test(this.$route.path)) {
        return 50
      } else if ((/\/customerOrder\/CustomerCheckout/).test(this.$route.path)) {
        return 75
      } else {
        return 100
      }
    }
  },
  components: {
    Footer
  }
}
</script>
<style scoped lang="scss">
.progress {
  overflow: inherit;
  background-color: grey;
  margin-top: 56px;
}
.progress-content{
  height: 15px;
  background-color: yellow;
  transition: all .5s;
}
</style>
