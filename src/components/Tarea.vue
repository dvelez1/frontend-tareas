<template>
  <div>
    <h1 class="display-4 text-center">Listado de Tareas</h1>
    <hr />
    <div class="row">
      <div class="col-lg-8 offset-lg-2">
        <div class="row">
          <div class="card mt-4">
            <div class="card-body">
              <div class="input-group">
                <!-- Bing property data.tarea to input textbox -->
                <input type="text" v-model="tarea"
                  class="text form-control form-control-lg" placeholder="Agregar Tarea"/>
                <div class="input-group-append">
                  <button v-on:click="agregarTarea()" 
                      class="btn-success btn-lg">Agregar</button>
                </div>
              </div>
              <br>
              <!--listTareas -> Property on Data Layer  -->
              <h5 v-if="listTareas.length == 0" >No hay tareas para realizar</h5>

              <ul class="list-group">
                <!-- Data Layer ->tarea: representa mi propiedad tarea / listTareas = Propiedad tipo Lista  -->
                <li v-for="(tarea, index) of listTareas" :key="index"
                    class="list-group-item d-flex justify-content-between">
                    <!-- See below if statement, when tarea.estado equal to true, apply color (class: text-success) -->
                  <span class="cursor" v-bind:class="{'text-success' : tarea.estado}"
                    v-on:click="editarTarea(tarea, index)">
                    <!-- See below if to exchange icons based on tarea.estado -->
                    <i v-bind:class="[tarea.estado ? 'fas fa-check-circle' : 'far fa-circle']"></i>
                  </span>
                  <!-- Constante objeto tarea con propiedades (Nombe ->Que a su ves es la prop tarea & propiedad estado) -->
                  {{ tarea.nombre }}
                  <span class="text-danger cursor" v-on:click="eliminarTarea(index)">
                    <i class="fas fa-trash-alt"></i>
                  </span>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Tarea",
  data() {
    return {
      tarea: "",
      listTareas: [],
    };
  },
  methods: {
    agregarTarea() {
      const tarea = {
        nombre: this.tarea,
        estado: false,
      };
      this.listTareas.push(tarea);
      this.tarea = "";
    },
    eliminarTarea(index) {
      this.listTareas.splice(index, 1);
    },
    editarTarea(tarea, index){
      this.listTareas[index].estado = !tarea.estado;
    }
  },
};
</script>

<style  scoped>
.cursor {
  cursor: pointer;
}
</style>