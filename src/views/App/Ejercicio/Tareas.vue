<template>
  <b-container fluid>
    <b-row>
        <b-col md="12">
          <iq-card>
            <template v-slot:headerTitle>
              <h4 class="card-title mt-3">Control de tareas</h4>
              <div class="iq-search-bar mt-2">
                <div class="row">
                  <div class="col-sm">
                    <b-form action="#" class="searchbox">
                    </b-form>
                  </div>
                  <div class="col-sm">
                  </div>
                </div>
              </div>
            </template>
            <template v-slot:body>
              <b-tabs>
                <b-tab title="Tareas" active>
                  <div class="d-flex justify-content-end mb-3">
                    <b-button variant="primary" v-b-modal.modal-1>Agregar Nueva tarea</b-button>
                  </div>
                  <table id="miTabla" class="display">
                    <thead>
                      <tr>
                          <th>ID</th>
                          <th>Nombre</th>
                          <th>Descripción</th>
                          <th>Curso</th>
                          <th>Fecha de Entrega</th>
                          <th>Estado</th>
                          <th>Acciones</th>
                          <th>Cambiar estado</th>
                      </tr>
                  </thead>
                  <tbody>
                      <tr v-for="tarea in datosPost" :key="tarea.id">
                          <td v-text="tarea.id"></td>
                          <td v-text="tarea.nombre"></td>
                          <td v-text="tarea.descripcion"></td>
                          <td v-text="tarea.curso"></td>
                          <td v-text="tarea.fecha_entrega"></td>
                          <td>
                            <template>
                              <span v-if="tarea.estado == 1" @click="postEstado(1, tarea.id)" class="badge badge-secondary">Pendiente</span>
                              <span v-if="tarea.estado == 2" @click="postEstado(2, tarea.id)" class="badge badge-danger">Cancelada</span>
                              <span v-if="tarea.estado == 3" @click="postEstado(3, tarea.id)" class="badge badge-success">Completada</span>
                            </template>
                          </td>
                          <td>
                              <b-button variant="primary" @click="setData(tarea)">
                                  <i class="fas fa-pencil-alt"></i>
                              </b-button>
                              &nbsp;
                              <b-button variant="primary" @click="modalEliminar(tarea)">
                                  <i class="fas fa-trash-alt"></i>
                              </b-button>
                          </td>
                          <td>
                              <template>
                                  <b-button v-if="tarea.estado == 2 || tarea.estado == 3" variant="secondary" @click="postEstado(1, tarea.id)">Pendiente</b-button>
                                  &nbsp;
                                  <b-button v-if="tarea.estado == 1 || tarea.estado == 3" variant="danger" @click="postEstado(2, tarea.id)">Cancelar</b-button>
                                  &nbsp;
                                  <b-button v-if="tarea.estado == 1 || tarea.estado == 2" variant="success" @click="postEstado(3, tarea.id)">Completar</b-button>
                              </template>
                          </td>
                      </tr>
                  </tbody>

                </table>
                </b-tab>
              </b-tabs>
            </template>
          </iq-card>
        </b-col>
    </b-row>
    <b-modal id="modal-1" ref="modal-1" size="xl" title="Ingreso de tareas">
        <form>
            <div class="row">
                <div class="col-md-6 mb-3">
                    <label for="NombreTarea">Nombre de la tarea</label>
                    <input
                        required
                        type="text"
                        class="form-control"
                        id="nombre"
                        v-model.trim="$v.form.nombre.$model"
                        :state="!$v.form.nombre.$error"
                        placeholder="Ingresar nombre de la tarea"
                    >
                </div>
                <div class="col-md-6 mb-3">
                    <label for="Descripcion">Descripción</label>
                    <input
                        required
                        class="form-control"
                        id="descripcion"
                        type="text"
                        v-model.trim="$v.form.descripcion.$model"
                        :state="!$v.form.descripcion.$error"
                        placeholder="Ingrese descripción"
                    >
                </div>
            </div>
            <div class="row">
                <div class="col-md-6 mb-3">
                    <label for="curso">Curso</label>
                    <input
                        required
                        type="text"
                        class="form-control"
                        id="curso"
                        v-model.trim="$v.form.curso.$model"
                        :state="!$v.form.curso.$error"
                        placeholder="Ingresar curso de la tarea"
                    >
                </div>
                <div class="col-md-6 mb-3">
                    <label for="fecha_entrega">Fecha de Entrega</label>
                    <input
                        required
                        class="form-control"
                        id="fecha_entrega"
                        type="date"
                        v-model.trim="$v.form.fecha_entrega.$model"
                        :state="!$v.form.fecha_entrega.$error"
                        placeholder="Ingrese fecha de entrega"
                    >
                </div>
            </div>
        </form>
        <template #modal-footer="{}">
            <b-button variant="primary" @click="onSave(), $bvModal.hide('modal-1')">Guardar</b-button>
            <b-button variant="danger" @click="$bvModal.hide('modal-1')">Cancelar</b-button>
        </template>
    </b-modal>

    <b-modal id="modal-2" ref="modal-2" size="xl" title="Edición de tarea">
        <form>
            <div class="row">
                <div class="col-md-6 mb-3">
                    <label for="NombreTarea">Nombre de la tarea</label>
                    <input
                        required
                        type="text"
                        class="form-control"
                        id="nombre"
                        v-model.trim="$v.form.nombre.$model"
                        :state="!$v.form.nombre.$error"
                        placeholder="Ingresar nombre de la tarea"
                    >
                </div>
                <div class="col-md-6 mb-3">
                    <label for="Descripcion">Descripción</label>
                    <input
                        required
                        class="form-control"
                        id="descripcion"
                        type="text"
                        v-model.trim="$v.form.descripcion.$model"
                        :state="!$v.form.descripcion.$error"
                        placeholder="Ingrese descripción"
                    >
                </div>
            </div>
            <div class="row">
                <div class="col-md-6 mb-3">
                    <label for="curso">Curso</label>
                    <input
                        required
                        type="text"
                        class="form-control"
                        id="curso"
                        v-model.trim="$v.form.curso.$model"
                        :state="!$v.form.curso.$error"
                        placeholder="Ingresar curso de la tarea"
                    >
                </div>
                <div class="col-md-6 mb-3">
                    <label for="fecha_entrega">Fecha de Entrega</label>
                    <input
                        required
                        class="form-control"
                        id="fecha_entrega"
                        type="date"
                        v-model.trim="$v.form.fecha_entrega.$model"
                        :state="!$v.form.fecha_entrega.$error"
                        placeholder="Ingrese fecha de entrega"
                    >
                </div>
            </div>
        </form>
        <template #modal-footer="{}">
            <b-button variant="primary" @click="onUpdate(), $bvModal.hide('modal-2')">Editar</b-button>
            <b-button variant="danger" @click="$bvModal.hide('modal-2')">Cancelar</b-button>
        </template>
    </b-modal>

    <b-modal id="modal-3" ref="modal-3" title="Eliminar tarea">
        <h6 class="my-4">
            ¿Desea eliminar la tarea?
        </h6>
        <template #modal-footer="{}">
            <b-button
                type="submit"
                variant="primary"
                @click="eliminarRegistro(datos), $bvModal.hide('modal-3')">Eliminar</b-button>
            <b-button variant="danger" @click="$bvModal.hide('modal-3')">Cancelar</b-button>
        </template>
    </b-modal>

  </b-container>
</template>
<script>
import { xray } from '../../../config/pluginInit'
import { laravelUrl } from '../../../config/constant'
import axios from 'axios'
import useVuelidate from '@vuelidate/core'
import { required } from '@vuelidate/validators'

export default {
  name: 'Ejercicio',
  setup () {
    return { $v: useVuelidate() }
  },
  components: {
  },
  data () {
    return {
      search: '',
      datosPost: [],
      form: {
        nombre: '',
        descripcion: '',
        curso: '',
        fecha_entrega: '',
        estado: 1
      },
      apiBase: laravelUrl + '/tareas/getTodos'
    }
  },
  mounted () {
    xray.index()
    axios.get(laravelUrl + '/tareas/getTodos').then((response) => {
      this.datosPost = response.data
    })
  },
  beforeMount () {
  },
  validations () {
    return {
      form: {
        nombre: { required },
        descripcion: { required },
        curso: { required },
        fecha_entrega: { required },
        estado: { required }
      }
    }
  },
  methods: {
    modalEliminar (datos) {
      this.datos = datos
      this.$refs['modal-3'].show()
    },
    setData (datos) {
      this.datos = datos
      this.form.id = datos.id
      this.form.nombre = datos.nombre
      this.form.descripcion = datos.descripcion
      this.form.curso = datos.curso
      this.form.fecha_entrega = datos.fecha_entrega
      this.form.estado = datos.estado
      this.$refs['modal-2'].show()
    },
    modalActualizar (datos) {
      this.form = datos
      this.$bvModal.show('modal-2')
    },
    onUpdate () {
      const me = this
      axios.put(laravelUrl + '/tareas/' + parseInt(me.form.id), {
        nombre: me.form.nombre,
        descripcion: me.form.descripcion,
        curso: me.form.curso,
        fecha_entrega: me.form.fecha_entrega,
        estado: me.form.estado
      })
        .then((response) => {
          me.refresh()
          this.$v.$reset()
          this.$refs['modal-2'].hide()
          this.form.id = ''
          this.form.nombre = ''
          this.form.descripcion = ''
          this.form.curso = ''
          this.form.fecha_entrega = ''
          this.form.estado = 1
        })
        .catch((error) => {
          console.error('Error!', error)
        })
    },
    refresh () {
      axios.get(laravelUrl + '/tareas/getTodos').then((response) => {
        this.datosPost = response.data
      })
    },
    postEstado (accion, datosid) {
      const me = this
      if (accion === 1) {
        axios.put(laravelUrl + '/tareas/state/' + datosid, {
          estado: 1
        })
          .then((response) => {
            me.refresh()
          })
          .catch((error) => {
            console.error('Error!', error)
          })
      } else if (accion === 2) {
        axios.put(laravelUrl + '/tareas/state/' + datosid, {
          estado: 2
        })
          .then((response) => {
            me.refresh()
          })
          .catch((error) => {
            console.error('Error!', error)
          })
      } else if (accion === 3) {
        axios.put(laravelUrl + '/tareas/state/' + datosid, {
          estado: 3
        })
          .then((response) => {
            me.refresh()
          })
          .catch((error) => {
            console.error('Error!', error)
          })
      }
    },
    eliminarRegistro (datos) {
      axios.delete(laravelUrl + '/tareas/' + datos.id).then((response) => {
        console.log(datos.id)
        this.refresh()
      })
        .catch((error) => {
          console.error('Error!', error)
        })
      this.refresh()
    },
    onSave () {
      const me = this
      axios.post(laravelUrl + '/tareas/', {
        nombre: me.form.nombre,
        descripcion: me.form.descripcion,
        curso: me.form.curso,
        fecha_entrega: me.form.fecha_entrega,
        estado: 1
      })
        .then((response) => {
          me.refresh()
          this.$v.$reset()
          this.$refs['modal-1'].hide()
          this.form.id = ''
          this.form.nombre = ''
          this.form.descripcion = ''
          this.form.curso = ''
          this.form.fecha_entrega = ''
          this.form.estado = 1
        })
        .catch((error) => {
          console.error('Error!', error)
        })
    }
  }
}
</script>
  <style>
  table {
      width: 100%;
      border-collapse: collapse;
  }

  /* Estilo para las celdas de encabezado */
  th {
      background-color: #f2f2f2;
  }

  /* Estilo para las celdas de datos */
td, th {
    border: 1px solid #ddd;
    padding: 8px;
    text-align: left;
}
</style>
