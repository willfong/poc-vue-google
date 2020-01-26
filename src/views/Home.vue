<template>
  <div>
    <h1>POC // Vue + Google OAuth2</h1>
    <button @click="handleClickGetAuth" :disabled="!isInit">get auth code</button>
    <button @click="handleClickSignIn" v-if="!isSignIn" :disabled="!isInit">signIn</button>
    <button @click="handleClickSignOut" v-if="isSignIn" :disabled="!isInit">signOout</button>
  </div>
</template>

<script>
export default {
  name: 'test',
  data () {
    return {
      isInit: false,
      isSignIn: false
    }
  },

  methods: {
    handleClickGetAuth(){
      this.$gAuth.getAuthCode()
      .then(authCode => {
        // On success
        console.log("Google Auth Code: " + authCode) // eslint-disable-line no-console
      })
      .then(response => {
        // And then
        console.log("Google Response: " + response) // eslint-disable-line no-console
      })
      .catch(error => {
        console.log("Google Response: " + error) // eslint-disable-line no-console
      })
    },

    handleClickSignIn(){
      this.$gAuth.signIn()
      .then(user => {
        // On success do something, refer to https://developers.google.com/api-client-library/javascript/reference/referencedocs#googleusergetid
        console.log(user) // eslint-disable-line no-console
        this.isSignIn = this.$gAuth.isAuthorized
      })
      .catch(error  => {
        // On fail do something
        console.log(error) // eslint-disable-line no-console
      })
    },

    handleClickSignOut(){
      this.$gAuth.signOut()
      .then(() => {
        // On success do something
        this.isSignIn = this.$gAuth.isAuthorized
      })
      .catch(error  => {
        // On fail do something
        console.log(error) // eslint-disable-line no-console
      })
    }
  },
  mounted(){
    let that = this
    let checkGauthLoad = setInterval(function(){
      that.isInit = that.$gAuth.isInit
      that.isSignIn = that.$gAuth.isAuthorized
      if(that.isInit) clearInterval(checkGauthLoad)
    }, 1000);
  }
  
}
</script>
