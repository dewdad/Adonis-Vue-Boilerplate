<template lang='pug'>
  v-app-bar(app hide-on-scroll)
    div.header-logo(style='flex: 1')
      a(v-on:click="$router.push('/Home')")
        img.logo(:src='logo' height='50px')
    span(style='flex:3') {{header}} 
    v-spacer
    v-tabs(style='flex:2' right hide-slider)
      v-tab(v-for='link in headerLinks' :key='link.name')
        router-link(v-if='visible(link)' :to='link.target || link.name')
          v-btn.btn-primary(v-if='linkType === "button"') {{link.name}}
          span(v-else) {{link.name}}
      UserMenu(v-if='payload.userid')
</template>

<script>
import config from '@/config.js'
import UserMenu from '@/components/UserMenu.vue'

export default {
  data () {
    return {
      linkType: 'text',
      publicHeaders: [
        {name: 'About', public: true},
        {name: 'Login', public: true},
        {name: 'Register', public: true, target: 'SignUp'}
      ],
      privateHeaders: [
        {name: 'Logout', private: true},
        {name: 'Admin', private: true, access: 'admin', target: 'admin'},
        {name: 'Data', private: true},
        {name: 'Home', private: true}
      ]
    }
  },
  components: {
    UserMenu
  },
  computed: {
    header: function () {
      return config.header || ''
    },
    logo: function () {
      var file = config.headerLogo || 'logo.svg'
      return 'images/' + file
    },
    headerLinks: function () {
      if (this.payload && this.payload.userid) {
        return this.privateHeaders
      } else {
        return this.publicHeaders
      }
    },
    payload: function () {
      return this.$store.getters.payload || {}
    }
  },
  methods: {
    visible: function (link) {
      if (link.access === 'admin') {
        return (this.payload.access === 'admin')
      } else {
        return true
      }
    }
  },
  watch: {
    payload: function () {
      console.log('New payload: ' + JSON.stringify(this.payload))
    }
  }
}
</script>
<style>
.v-toolbar__content {
  align-items: center;
}
</style>
