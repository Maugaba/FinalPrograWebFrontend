<template>
  <b-container fluid>
    <b-row>
      <b-col md="12">
        <iq-card>
          <template v-slot:headerTitle>
            <h4 class="card-title mt-3">Vehiculos</h4>
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
              <b-tab title="Vehiculo" active>
                <div class="d-flex justify-content-end mb-3">
                  <b-button variant="primary" v-b-modal.modal-1>AGREGAR NUEVO</b-button>
                </div>
                <table id="miTabla" class="display">
                  <thead>
                      <tr>
                          <th># Placa</th>
                          <th>VIN</th>
                          <th>No. chasis</th>
                          <th>Nombre propietario</th>
                          <th>Tipo de placa</th>
                          <th>Estado</th>
                          <th>Acciones</th>
                      </tr>
                  </thead>
                  <tbody>
                    <tr v-for="vehiculo in vehiculos" :key="vehiculo.id">
                        <td v-text="vehiculo.NumerodePlaca"></td>
                        <td v-text="vehiculo.VIN"></td>
                        <td v-text="vehiculo.NumeroChasis"></td>
                        <td v-text="vehiculo.NombrePropietario"></td>
                        <td>
                          <template v-for="tipoPlaca in tiposplacas">
                            <template v-if="tipoPlaca.id === vehiculo.tiposdeplaca_id ">
                              {{ tipoPlaca.NombreTipodePlaca }}
                            </template>
                          </template>
                        </td>
                        <td>
                            <template v-if="vehiculo.Estado === '1'">
                              ACTIVO
                            </template>
                            <template v-else>
                              INACTIVO
                            </template>
                        </td>
                        <td>
                          <template>
                            <b-button @click="editar(vehiculo.id)" variant="primary">Editar</b-button>
                            &nbsp;
                          </template>
                          <template v-if="vehiculo.Estado === '1'">
                                <button class="btn btn-success" @click="vehiculoEstado(1, vehiculo.id)">
                                    DESACTIVAR
                                </button>
                          </template>
                          <template v-else>
                              <button class="btn btn-danger" @click="vehiculoEstado(2, vehiculo.id)">
                                  ACTIVAR
                              </button>
                          </template>
                          <template>
                            &nbsp;
                            <button class="btn btn-danger" @click="eliminar(vehiculo.id)">
                                ELIMINAR
                            </button>
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
    <b-modal id="modal-1" ref="modal-1" size="xl" title="Bienvenido al modal de ingreso de vehiculo">
      <form>
        <div class="row">
          <div class="col-md-6 mb-3">
            <label for="numPlaca">Numero de placa</label>
            <input
              required
              type="text"
              class="form-control"
              id="numPlaca"
              v-model.trim="$v.form.NumerodePlaca.$model"
              :state="!$v.form.NumerodePlaca.$error"
              placeholder="Ingresar Numero de placa"
            >
          </div>
          <div class="col-md-6 mb-3">
            <label for="VIN">VIN</label>
            <input
              required
              type="text"
              class="form-control"
              id="VIN"
              v-model.trim="$v.form.VIN.$model"
              :state="!$v.form.VIN.$error"
              placeholder="Ingresar VIN"
            >
          </div>
          <div class="col-md-6 mb-3">
            <label for="numChasis">Numero de chasis</label>
            <input
              required
              type="text"
              class="form-control"
              id="numChasis"
              v-model.trim="$v.form.NumeroChasis.$model"
              :state="!$v.form.NumeroChasis.$error"
              placeholder="Ingresar Numero de chasis"
            >
          </div>
          <div class="col-md-6 mb-3">
            <label for="nomPropietario">Nombre de propietario</label>
            <input
              required
              type="text"
              class="form-control"
              id="nomPropietario"
              v-model.trim="$v.form.NombrePropietario.$model"
              :state="!$v.form.NombrePropietario.$error"
              placeholder="Ingresar Nombre de propietario"
            >
          </div>
          <div class="col-md-6 mb-3">
            <label for="idTipoPlaca">Tipo placa</label>
            <select
              required
              class="form-control"
              id="idTipoPlaca"
              v-model.trim="$v.form.tiposdeplaca_id.$model"
              :state="!$v.form.tiposdeplaca_id.$error"
            >
            <option v-for="tipoPlaca in tiposplacas" :key="tipoPlaca.id" :value="tipoPlaca.id">
              {{ tipoPlaca.NombreTipodePlaca }}
            </option>
            </select>
          </div>
        </div>
      </form>
      <template #modal-footer="{}">
        <b-button  variant="primary" @click="onValidate('save')"
          >Guardar</b-button
        >
        <b-button variant="danger" @click="closeModal('save')"
          >Cancelar</b-button
        >
      </template>
    </b-modal>
    <b-modal id="modal-2" ref="modal-2" size="xl" title="Bienvenido al modal de edicion de vehiculo">
      <form>
        <div class="row">
          <div class="col-md-6 mb-3">
            <label for="numPlaca">Numero de placa</label>
            <input
              required
              type="text"
              class="form-control"
              id="numPlaca"
              v-model.trim="$v.form.NumerodePlaca.$model"
              :state="!$v.form.NumerodePlaca.$error"
              placeholder="Ingresar Numero de placa"
            >
          </div>
          <div class="col-md-6 mb-3">
            <label for="VIN">VIN</label>
            <input
              required
              type="text"
              class="form-control"
              id="VIN"
              v-model.trim="$v.form.VIN.$model"
              :state="!$v.form.VIN.$error"
              placeholder="Ingresar VIN"
            >
          </div>
          <div class="col-md-6 mb-3">
            <label for="numChasis">Numero de chasis</label>
            <input
              required
              type="text"
              class="form-control"
              id="numChasis"
              v-model.trim="$v.form.NumeroChasis.$model"
              :state="!$v.form.NumeroChasis.$error"
              placeholder="Ingresar Numero de chasis"
            >
          </div>
          <div class="col-md-6 mb-3">
            <label for="nomPropietario">Nombre de propietario</label>
            <input
              required
              type="text"
              class="form-control"
              id="nomPropietario"
              v-model.trim="$v.form.NombrePropietario.$model"
              :state="!$v.form.NombrePropietario.$error"
              placeholder="Ingresar Nombre de propietario"
            >
          </div>
          <div class="col-md-6 mb-3">
            <label for="idTipoPlaca">Tipo placa</label>
            <select
              required
              class="form-control"
              id="idTipoPlaca"
              v-model.trim="$v.form.tiposdeplaca_id.$model"
              :state="!$v.form.tiposdeplaca_id.$error"
            >
            <option v-for="tipoPlaca in tiposplacas" :key="tipoPlaca.id" :value="tipoPlaca.id">
              {{ tipoPlaca.NombreTipodePlaca }}
            </option>
            </select>
          </div>
        </div>
      </form>
      <template #modal-footer="{}">
        <b-button  variant="primary" @click="onValidate('update')"
          >Editar</b-button
        >
        <b-button variant="danger" @click="closeModal('update')"
          >Cancelar</b-button
        >
      </template>
    </b-modal>
    <b-modal id="modal-3" ref="modal-3" title="Eliminar vehiculo">
      <h6 class="my-4">
        ¿Desea eliminar el vehiculo con placa: {{ form.NumerodePlaca }} ?
      </h6>
      <template #modal-footer="{}">
        <b-button
          type="submit"
          variant="primary"
          @click="Eliminar(form.id)
                  $bvModal.hide('modal-3')"
          >Eliminar</b-button
        >
        <b-button variant="danger" @click="$bvModal.hide('modal-3')"
          >Cancelar</b-button
        >
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
  name: 'Vehiculos',
  setup () {
    return { $v: useVuelidate() }
  },
  components: {

  },
  data () {
    return {
      form: {
        id: '',
        NumerodePlaca: '',
        VIN: '',
        NumeroChasis: '',
        NombrePropietario: '',
        tiposdeplaca_id: null,
        Estado: '1'
      },
      vehiculos: [],
      tiposplacas: [],
      vehiculoIndividual: [],
      nameVehiculo: '',
      apiBase: laravelUrl + '/vehiculos',
      typeOptions: []
    }
  },
  mounted () {
    xray.index()
    this.refresh()
  },
  beforeMount () {
  },
  validations () {
    return {
      form: {
        NumerodePlaca: { required },
        VIN: { required },
        NumeroChasis: { required },
        NombrePropietario: { required },
        tiposdeplaca_id: { required },
        Estado: { }
      }
    }
  },
  methods: {
    vehiculoEstado (accion, vehiculoid) {
      const me = this
      if (accion === 1) {
        axios.put(laravelUrl + '/vehiculos/state/' + vehiculoid, {
          Estado: 0 })
          .then((response) => {
            me.refresh()
          })
          .catch((error) => {
            // this.errorMessage = error.message;
            console.error('Error!', error)
          })
      } else if (accion === 2) {
        axios.put(laravelUrl + '/vehiculos/state/' + vehiculoid, {
          Estado: 1 })
          .then((response) => {
            me.refresh()
          })
          .catch((error) => {
            // this.errorMessage = error.message;
            console.error('Error!', error)
          })
      }
    },
    openModal (modal, action) {
      switch (modal) {
        case 'save': {
          this.$v.$reset()
          this.form.id = ''
          this.form.NumerodePlaca = ''
          this.form.VIN = ''
          this.form.NumeroChasis = ''
          this.form.NombrePropietario = ''
          this.form.tiposdeplaca_id = null
          this.form.Estado = '1'
          break
        }
      }
    },
    closeModal (action) {
      switch (action) {
        case 'save': {
          this.$v.$reset()
          this.$refs['modal-1'].hide()
          this.form.id = ''
          this.form.NumerodePlaca = ''
          this.form.VIN = ''
          this.form.NumeroChasis = ''
          this.form.NombrePropietario = ''
          this.form.tiposdeplaca_id = null
          this.form.Estado = '1'
          break
        }
        case 'update': {
          this.$v.$reset()
          this.$refs['modal-2'].hide()
          this.form.id = ''
          this.form.NumerodePlaca = ''
          this.form.VIN = ''
          this.form.NumeroChasis = ''
          this.form.NombrePropietario = ''
          this.form.tiposdeplaca_id = null
          this.form.Estado = '1'
          break
        }
      }
    },
    onValidate (action) {
      this.$v.$touch()
      if (this.$v.$error !== true) {
        if (action === 'save') {
          this.onSave()
        } else if (action === 'update') {
          this.onUpdate()
        }
      }
    },
    setData (data) {
      this.form.user = data.user
      this.form.state = data.state
      this.form.id = data.id
    },
    /* Guardar */
    onSave () {
      const me = this
      axios.post(laravelUrl + '/vehiculos/', {
        NumerodePlaca: me.form.NumerodePlaca,
        VIN: me.form.VIN,
        NumeroChasis: me.form.NumeroChasis,
        NombrePropietario: me.form.NombrePropietario,
        tiposdeplaca_id: parseInt(me.form.tiposdeplaca_id),
        Estado: me.form.Estado
      })
        .then((response) => {
          me.refresh()
          me.closeModal('save')
        })
        .catch((error) => {
          console.log(error)
        })
    },
    /* Guardar */
    onUpdate () {
      const me = this
      axios.put(laravelUrl + '/vehiculos/' + me.form.id, {
        NumerodePlaca: me.form.NumerodePlaca,
        VIN: me.form.VIN,
        NumeroChasis: me.form.NumeroChasis,
        NombrePropietario: me.form.NombrePropietario,
        tiposdeplaca_id: parseInt(me.form.tiposdeplaca_id)
      })
        .then((response) => {
          me.refresh()
          me.closeModal('update')
        })
        .catch((error) => {
          // this.errorMessage = error.message;
          console.error('Error!', error)
        })
    },
    makeQueryParams (sortOrder, currentPage, perPage) {
      return sortOrder[0]
        ? {
          criterio: sortOrder[0] ? sortOrder[0].sortField : 'createdAt',
          order: sortOrder[0] ? sortOrder[0].direction : 'desc',
          page: currentPage,
          limit: this.perPage,
          search: this.search,
          columna: this.columna.value
        }
        : {
          criterio: sortOrder[0] ? sortOrder[0].sortField : 'createdAt',
          order: sortOrder[0] ? sortOrder[0].direction : 'desc',
          page: currentPage,
          limit: this.perPage,
          search: this.search,
          columna: this.columna.value
        }
    },
    changePageSizes (perPage) {
      this.perPage = perPage
      this.$refs.vuetable.refresh()
    },
    searchChange (val) {
      this.search = val.toLowerCase()
      this.$refs.vuetable.refresh()
    },
    onPaginationData (paginationData) {
      this.from = paginationData.from
      this.to = paginationData.to
      this.total = paginationData.total
      this.lastPage = paginationData.last_page
      this.items = paginationData.data
      this.$refs.pagination.setPaginationData(paginationData)
    },
    onChangePage (page) {
      this.$refs.vuetable.changePage(page)
    },
    changeTypeSearch (columna) {
      this.columna = columna
    },
    mensaje (mensaje) {
      this.$refs['modal-1'].show()
      this.form.nombre = mensaje
    },
    refresh () {
      axios.get(laravelUrl + '/vehiculos/getTodos').then((response) => {
        this.vehiculos = response.data
        console.log(this.vehiculos)
      })
      axios.get(laravelUrl + '/tiposdeplaca/getTodos').then((response) => {
        this.tiposplacas = response.data
      })
    },
    editar (vehiculoId) {
      axios.get(laravelUrl + '/vehiculos/' + vehiculoId).then((response) => {
        this.vehiculoIndividual = response.data
        this.$v.$reset()
        this.$refs['modal-2'].show()
        this.form.id = this.vehiculoIndividual.id
        this.form.NumerodePlaca = this.vehiculoIndividual.NumerodePlaca
        this.form.VIN = this.vehiculoIndividual.VIN
        this.form.NumeroChasis = this.vehiculoIndividual.NumeroChasis
        this.form.NombrePropietario = this.vehiculoIndividual.NombrePropietario
        this.form.tiposdeplaca_id = this.vehiculoIndividual.tiposdeplaca_id
        this.form.Estado = this.vehiculoIndividual.Estado
      })
    },
    eliminar (teamid) {
      axios.get(laravelUrl + '/vehiculos/' + teamid).then((response) => {
        this.vehiculoIndividual = response.data
        this.$v.$reset()
        this.$refs['modal-3'].show()
        this.form.id = this.vehiculoIndividual.id
        this.form.NumerodePlaca = this.vehiculoIndividual.NumerodePlaca
      })
    },
    Eliminar (teamid) {
      axios.delete(laravelUrl + '/vehiculos/' + teamid).then((response) => {
        this.refresh()
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
