<template>
  <div style="width: 100%; padding: 0; margin: 0;">
    <v-row align="center" justify="center">
      <h1>CRUD Empleados</h1>
    </v-row>
    <v-row
      align="enter"
      justify="end"
      style="margin-top: 10px; margin-bottom: 10px;"
    >
      <v-btn color="primary" @click="crearEmpleado">
        <v-icon> mdi-account </v-icon>
        <span style="text-transform: none;">
          Crear Empleado
        </span>
      </v-btn>
    </v-row>
    <v-row align="center" justify="center">
      <v-data-table
        style="width: 100%;"
        :headers="headers"
        :items="empleados"
      >
        <template #[`item.acciones`]="{ item }">
          <v-tooltip bottom color="orange">
            <template #activator="{on, attrs}">
              <v-btn
                icon
                color="warning"
                v-bind="attrs"
                :disabled="item.perfil === 'admin'"
                @click="updateEmpleado(item)"
                v-on="on"
              >
                <v-icon>
                  mdi-update
                </v-icon>
              </v-btn>
            </template>
            <span>
              Actualizar al Empleado: {{ item.nombre }}
            </span>
          </v-tooltip>
          <v-tooltip bottom color="red">
            <template #activator="{on, attrs}">
              <v-btn
                icon
                color="red"
                v-bind="attrs"
                :disabled="item.perfil === 'admin'"
                @click="deleteEmpleado(item.id)"
                v-on="on"
              >
                <v-icon>
                  mdi-delete
                </v-icon>
              </v-btn>
            </template>
            <span>
              Borrar al Empleado: {{ item.nombre }}
            </span>
          </v-tooltip>
        </template>
      </v-data-table>
    </v-row>
    <borrar-empleado
      v-if="showBorrar"
      :id-empleado="idEmpleado"
      @close="destruyeComponente"
      @actualiza="actualizaTabla"
    />
    <crear-empleado
      v-if="showCrear"
      :flag-modify="flag"
      :employee="flag ? auxEmpleado: null"
      @close="destruyeCrear"
      @actualiza="actualizaTabla"
    />
  </div>
</template>

<script>
import borrarEmpleado from '~/components/empleados/borrarEmpleado.vue'
import crearEmpleado from '~/components/empleados/crearEmpleado.vue'
export default {
  components: {
    borrarEmpleado,
    crearEmpleado
  },
  data () {
    return {
      empleados: [],
      headers: [
        {
          text: 'Nombre',
          align: 'center',
          sortable: true,
          value: 'nombre'
        },
        {
          text: 'A. Paterno',
          align: 'center',
          sortable: true,
          value: 'apaterno'
        },
        {
          text: 'A. Materno',
          align: 'center',
          sortable: true,
          value: 'amaterno'
        },
        {
          text: 'Ciudad',
          align: 'center',
          sortable: true,
          value: 'ciudad'
        },
        {
          text: 'Estado',
          align: 'center',
          sortable: true,
          value: 'estado'
        },
        {
          text: 'Direccion',
          align: 'center',
          sortable: true,
          value: 'direccion'
        },
        {
          text: 'Telefono',
          align: 'center',
          sortable: true,
          value: 'telefono'
        },
        {
          text: 'Perfil',
          align: 'center',
          sortable: true,
          value: 'perfil'
        },
        {
          text: 'Acciones',
          align: 'center',
          sortable: false,
          value: 'acciones'
        }
      ],
      showBorrar: false,
      idEmpleado: null,
      showCrear: false,
      auxEmpleado: {},
      flag: false
    }
  },
  mounted () {
    this.loadEmpleados()
  },
  methods: {
    async loadEmpleados () {
      const response = await this.$axios.get('/get-all')
      if (response.data.success === true) {
        // this.empleados = response.data.message
        // const res = await response.json()
        this.empleados = []
        response.data.message.forEach((item) => {
          this.empleados.push(item)
        })
      }
      console.log('@@@ response => ', response)
    },
    deleteEmpleado (id) {
      this.idEmpleado = id
      this.showBorrar = true
    },
    destruyeComponente () {
      this.showBorrar = false
      this.idEmpleado = null
    },
    actualizaTabla () {
      this.showCrear = false
      this.showBorrar = false
      this.idEmpleado = null
      this.flag = false
      this.loadEmpleados()
    },
    destruyeCrear () {
      this.showCrear = false
      this.flag = false
    },
    crearEmpleado () {
      this.showCrear = true
    },
    updateEmpleado (item) {
      this.auxEmpleado = item
      this.flag = true
      this.showCrear = true
    }
  }
}
</script>

<style scoped>
</style>
