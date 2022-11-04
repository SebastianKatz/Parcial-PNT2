<template>

  <section class="src-componentes-formulario">
    <div class="jumbotron">
      <h2>Formulario de Importes</h2>
      <hr>

      <vue-form :state="formstate" @submit.prevent="enviar()">
        
        <!-- Campo nombre -->
        <validate tag="div">
          <label for="nombre">Nombre</label>
          <input
            type="text"
            id="nombre"
            class="form-control"
            autocomplete="off"
            v-model.trim="formData.nombre"
            name="nombre"
            required
            :minlength="nombreMinLength"
            :maxlength="nombreMaxLength"
          />
          <!-- mensajes de validación -->
          <field-messages name="nombre" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">
              Campo requerido
            </div>
            <div slot="minlength" class="alert alert-danger mt-1">
              Este campo debe poseer al menos {{ nombreMinLength }} caracteres.
            </div>
            <div slot="maxLength" class="alert alert-danger mt-1">
              Este campo debe poseer al menos {{ nombreMaxLength }} caracteres.
            </div>
          </field-messages>
        </validate>
        <br>

        <!-- Campo descripcion -->
        <validate tag="div">
          <label for="descripcion">Descripcion</label>
          <input
            type="text"
            id="descripcion"
            class="form-control"
            autocomplete="off"
            v-model.trim="formData.descripcion"
            name="descripcion"
            required
          />
          <!-- mensajes de validación -->
          <field-messages name="descripcion" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">
              Campo requerido
            </div>
          </field-messages>
        </validate>

        <br>
        
        <!-- Campo importe -->
        <validate tag="div">
          <label for="importe">Importe</label>
          <input type="number" id="importe" v-model.number="formData.importe" required name="importe" autocomplete="off" class="form-control" />
    
          <field-messages name="importe" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
          </field-messages>
        </validate>

        <br>


        <button class="btn btn-success my-3" :disabled="formstate.$invalid" type="submit">Enviar</button>
      </vue-form>
      <br>
      <hr>
      <label for="presupuesto"><i><b>Presupuesto:</b></i></label>
      <input type="number" id="presupuesto" v-model.number="presupuesto" @change="actualizarColor()" required name="presupuesto" autocomplete="off" class="form-control" placeholder="Ingrese el presupuesto" />

      <!-- Tabla para representar los datos ingresados -->
      <h2>Detalle de Gastos</h2>
      <br>

      <div v-if="gastos.length" class="table-responsive">
        <table class="table">
          <tr>
            <th>Nombre</th>
            <th>Descripcion</th>
            <th>Importe</th>
            <th>Fecha</th>
          </tr>
            <tr v-for="(gasto,index) in gastos" :key="index">
            <td>{{ gasto.nombre }}</td>
            <td>{{ gasto.descripcion }}</td>
            <td> $ {{gasto.importe}}</td>
            <td>{{ gasto.fecha }}</td>
          </tr>
          <tr :style="{color: this.colorTotal}">
            <td>
              <p>Total: </p>
            </td>
            <td>
              <p></p>
            </td>
            <td><p> $ {{ obtenerGastosTotales() }}</p></td>
          </tr>
        </table>
      </div>
      <h3 v-else class="alert alert-info">No hay gastos ingresados</h3>

    </div>
  </section>

</template>

<script>

  export default  {
    name: 'src-componentes-formulario',
    props: [],
    mounted () {

    },
    data () {
      return {
        formstate : {},
        formData : this.getInitialData(),
        nombreMinLength: 3,
        nombreMaxLength: 15,
        gastos : [],
        presupuesto: null,
        totalGastos: 0,
        colorTotal: 'black'
      }
    },
    methods: {
      getInitialData() {
        return {
          nombre : null,
          importe: null,
          descripcion: null
        }
      },
      enviar() {
        let gasto = {...this.formData}
        gasto.fecha = new Date().toLocaleString()

        console.log(gasto)
        this.gastos.push(gasto)

        this.formData = this.getInitialData()
        this.formstate._reset()
      },
      obtenerGastosTotales() {
        this.totalGastos = this.gastos.map(gasto => gasto.importe).reduce((sumatoria, gasto) => sumatoria + gasto, 0)
        if(this.totalGastos < 1000) {
          this.colorTotal = 'green'
        } else if(this.totalGastos >= 1000 && this.totalGastos <= 5000) {
          this.colorTotal = 'purple'
        } else {
          this.colorTotal = 'orange'
        }
        console.log(this.totalGastos)
        return this.totalGastos
      },
      actualizarColor() {
        if (this.totalGastos > this.presupuesto) {
          this.colorTotal = 'red'
        }
      }
    },
    computed: {
    }
}


</script>

<style scoped lang="css">
  .jumbotron {
    background-color: lightyellow;
    color: brown;
  }

  hr {
    background-color: #eee;
  }

  pre {
    color: white;
  }
</style>
