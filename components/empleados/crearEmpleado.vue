<template>
  <v-dialog
    v-model="showDialog"
    width="800"
  >
    <v-card>
      <v-card-title>
        {{ flagModify ? 'Modificar al Empleado' : 'Agregar un Empleado' }}
      </v-card-title>
      <v-card-text class="mt-4">
        <v-form ref="frmCreate">
          <v-row>
            <v-col cols="12">
              <v-text-field
                v-model="empleado.nombre"
                label="Nombre"
                placeholder="Nombre"
                dense
                filled
                outlined
                :rules="[regla.requerido]"
              />
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="6">
              <v-text-field
                v-model="empleado.apaterno"
                label="Apellido Paterno"
                placeholder="Apellido Paterno"
                dense
                filled
                outlined
                :rules="[regla.requerido]"
              />
            </v-col>
            <v-col cols="6">
              <v-text-field
                v-model="empleado.amaterno"
                label="Apellido Materno"
                placeholder="Apellido Materno"
                dense
                filled
                outlined
              />
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="12">
              <v-text-field
                v-model="empleado.direccion"
                label="Direccion"
                placeholder="Direccion"
                dense
                filled
                outlined
                :rules="[regla.requerido]"
              />
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="empleado.telefono"
                label="Telefono"
                placeholder="Telefono"
                dense
                filled
                outlined
                :rules="[regla.requerido]"
              />
            </v-col>
            <v-col cols="4">
              <v-text-field
                v-model="empleado.ciudad"
                label="Ciudad"
                placeholder="Ciudad"
                dense
                filled
                outlined
                :rules="[regla.requerido]"
              />
            </v-col>
            <v-col cols="4">
              <v-text-field
                v-model="empleado.estado"
                label="Estado"
                placeholder="Estado"
                dense
                filled
                outlined
                :rules="[regla.requerido]"
              />
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="empleado.correo"
                label="Correo"
                placeholder="Correo"
                dense
                filled
                outlined
                :rules="[regla.requerido, regla.correo]"
              />
            </v-col>
            <v-col cols="4">
              <v-text-field
                v-model="empleado.contrasena"
                type="password"
                label="Password"
                placeholder="Password"
                dense
                filled
                outlined
                :rules="[regla.requerido, regla.size]"
              />
            </v-col>
            <v-col cols="4">
              <v-combobox
                v-model="empleado.perfil"
                :items="perfiles"
                dense
                outlined
                filled
              />
            </v-col>
          </v-row>
        </v-form>
      </v-card-text>
      <v-card-actions>
        <v-row
          align="center"
          justify="center"
          class="pa-2 ma-2"
        >
          <v-btn color="gray" @click="closeDialog">
            <span style="text-transform: none;">
              Cancelar
            </span>
          </v-btn>
          <v-btn color="yellow" @click="operacion">
            <span style="text-transform: none;">
              {{ flagModify ? 'Modificar' : 'Agregar' }}
            </span>
          </v-btn>
        </v-row>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  props: {
    flagModify: { type: Boolean, default: false },
    employee: { type: Object, default: null }
  },
  data () {
    return {
      showDialog: true,
      empleado: {},
      perfiles: ['admin', 'rh', 'conta', 'sistemas'],
      regla: {
        requerido: val => (val || '').length > 0 || 'Este campo es requerido',
        size: val => (val || '').length > 6 || 'Este campo necesita 6 caracteres',
        correo: val => /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(val) || 'Correo debe ser valido'
      }
    }
  },
  mounted () {
    console.log('@@@ props => ', this.flagModify, this.employee)
    if (this.flagModify) {
      this.empleado = this.employee
      this.empleado.contrasena = ''
    } else {
      this.empleado.perfil = 'admin'
    }
  },
  methods: {
    operacion () {
      if (this.flagModify) {
        this.updateEmpleado()
      } else {
        this.crearEmpleado()
      }
    },
    closeDialog () {
      this.showDialog = false
      this.$emit('close')
    },
    async crearEmpleado () {
      const isValid = this.$refs.frmCreate.validate()
      if (isValid) {
        this.empleado.noempleado = new Date()
        const res = await this.$axios.post('/create', this.empleado)
        console.log('@@@ res => ', res)
        if (res.data.success) {
          this.$emit('actualiza')
        }
      }
    },
    async updateEmpleado () {
      const isValid = this.$refs.frmCreate.validate()
      if (isValid) {
        const res = await this.$axios.put(`/update/${this.empleado.id}`, this.empleado)
        console.log('@@@ res => ', res)
        if (res.data.success) {
          this.$emit('actualiza')
        }
      }
    }
  }
}
</script>
