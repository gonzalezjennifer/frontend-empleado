<template>
  <v-card elevation="0" color="pink darken-3" width="400" height="300" rounded>
    <v-card-title>
      <v-row algin="center" justify="center">
        <span style="color: black;">
          Bienvenido
        </span>
      </v-row>
    </v-card-title>
    <v-card-text class="mt-4 mb-4 pt-4 pb-4">
      <v-form ref="form">
        <div class="textFields">
          <v-row style="width: 100%;">
            <v-text-field v-model="usuario" solo :rules="size" />
          </v-row>
          <v-row style="width: 100%;">
            <v-text-field v-model="password" type="password" solo :rules="size" />
          </v-row>
        </div>
      </v-form>
    </v-card-text>
    <v-card-actions>
      <v-row align="center" justify="center">
        <v-btn elevation="0" color="pink-lighten-3" @click="loginBackend">
          <span style="text-transform: none; color: #880E4F;">
            Login
          </span>
        </v-btn>
      </v-row>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  data () {
    return {
      usuario: '',
      password: '',
      size: [
        v => v.trim().length !== 0 || 'No puede estar vacio'
      ]
    }
  },
  methods: {
    async loginBackend () {
      console.log('@@@ Variable', this.usuario, this.password)
      const isvalid = this.$refs.form.validate()
      if (isvalid) {
        const body = {
          correo: this.usuario,
          contrasena: this.password
        }
        // login con auth
        await this.$auth.loginWith('local', {
          data: body
        }).then((res) => {
          this.$emit('show-alert', {
            showAlert: true,
            color: 'green',
            type: 'success',
            message: 'Has iniciado sesion',
            icon: 'mdi-success'
          })
        }).catch((error) => {
          console.error('@@@ error => ', error)
        })
        /*
        this.$axios.post('/login', body)
          .then((res) => {
            console.log('@@ res => ', res)
          })
          .catch((error) => {
            console.error('@@@ error => ', error)
          }) */
      } else {
        this.$emit('show-alert', {
          showAlert: true,
          color: 'red',
          type: 'error',
          message: 'Los datos son incorrectos',
          icon: 'mdi-error'
        })
      }
    }
  }
}
</script>

<style scoped>
.textFields {
  width: 100%;
  margin: 10px;
}
</style>
