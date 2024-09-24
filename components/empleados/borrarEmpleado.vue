<template>
  <v-dialog
    v-model="showDialog"
    width="300"
    persistent
  >
    <v-card>
      <v-card-title>
        Borrar Empleado
      </v-card-title>
      <v-card-text>
        ¿Estas seguro?
      </v-card-text>
      <v-card-actions>
        <v-row
          align="center"
          justify="center"
          class="pa-2 ma-2"
        >
          <v-btn color="gray" @click="closeDialog">
            Cancelar
          </v-btn>
          <v-btn color="red" @click="borrarEmpleado">
            Borrar
          </v-btn>
        </v-row>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  props: {
    idEmpleado: { type: String, default: null }
  },
  data () {
    return {
      showDialog: true
    }
  },
  mounted () {
    console.log('@@@ id => ', this.idEmpleado)
  },
  methods: {
    closeDialog () {
      this.showDialog = false
      this.$emit('close')
    },
    async borrarEmpleado () {
      const res = await this.$axios.delete(`/borrar/${this.idEmpleado}`)
      if (res.data.success) {
        this.$emit('actualiza')
      }
      console.log('@@@ res => ', res)
    }
  }
}
</script>

<style scoped>
</style>
