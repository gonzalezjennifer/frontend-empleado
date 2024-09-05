<template>
  <v-app>
    <v-main>
      <v-alert
        v-if="showAlert"
        :color="color"
        :type="type"
        class="alert"
      >
        <v-icon v-if="icon">
          {{ icon }}
        </v-icon>
        {{ message }}
      </v-alert>
      <v-row align="center" justify="center" class="login">
        <Nuxt />
      </v-row>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      showAlert: false,
      color: '',
      type: '',
      message: '',
      icon: ''
    }
  },
  created () {
    this.$nuxt.$on('show-alert', (data) => {
      console.log('@@@ layout => ', data)
      this.showAlert = data.showAlert
      this.color = data.color
      this.type = data.type
      this.message = data.message
      this.icon = data.icon
      setTimeout(() => {
        this.showAlert = false
      }, 2000)
    })
  },
  beforeDestroy () {
    this.$nuxt.$off('show-alert')
  }
}
</script>

<style scoped>
.login {
  width: 100%;
  height: 100vh;
  background-color: #FCE4EC;
}
.alert {
  position: absolute;
  top: 20%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 1000;
  min-width: 300px;
}
</style>
