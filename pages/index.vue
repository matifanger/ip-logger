<template>
  <div style="width: 100vw; height: 100vh" class="parent">
    <v-progress-circular  indeterminate :size="100" color="grey"></v-progress-circular>
  </div>
</template>

<script>
import axios from 'axios'
  export default {
  head: {
    script: [
      {
        src: 'https://cdn.jsdelivr.net/gh/manuelmhtr/countries-and-timezones@latest/dist/index.min.js'
      }
    ],
  },
    data() {
      return {
        data: {
          ip: '',
          navigator: '',
          system: '',
        },
      }
    },
    mounted() {

      axios.get('https://httpbin.org/ip')
      .then(res => {
      axios.get('https://ipapi.co/' + res.data.origin + '/json/')
      .then(res => {
          this.data.ip = res.data
        })
      
      .then(res => {
      var navigator = {}

      navigator.appCodeName = window.navigator.appCodeName
      navigator.appVersion = window.navigator.appVersion
      navigator.language = window.navigator.language
      navigator.languages = window.navigator.languages
      navigator.platform = window.navigator.platform
      navigator.product = window.navigator.product
      navigator.productSub = window.navigator.productSub
      navigator.userAgent = window.navigator.userAgent
      navigator.vendor = window.navigator.vendor

      this.data.navigator = navigator

      // ! console.log(window.navigator.plugins)

      var system = {}

      system.date = new Date().toString(),
      system.locale = Intl.DateTimeFormat().resolvedOptions().locale,
      system.timeZone = Intl.DateTimeFormat().resolvedOptions().timeZone,
      system.timeZoneoffset = new Date().getTimezoneOffset()
      system.language = navigator.language
      system.languages = navigator.languages

      this.data.system = system
      }).then(res => {
        
    this.$fire.firestore.collection("logs").doc().set({
      data: this.data
      })
    })
    })

    },
    methods: {
    }
  }
</script>

<style lang="scss" scoped>
.parent {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>