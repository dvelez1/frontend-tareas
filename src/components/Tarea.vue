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
                <input
                  type="text"
                  v-model="tarea"
                  class="text form-control form-control-lg"
                  placeholder="Agregar Tarea"
                />
                <div class="input-group-append">
                  <button
                    v-on:click="agregarTarea()"
                    class="btn-success btn-lg"
                  >
                    Agregar
                  </button>
                </div>
              </div>
              <br />
              <div class="text-center">
                <!-- Spinner will triger when variable loading is true / otherwise will be hidden -->
                <div
                  v-if="loading"
                  class="spinner-grow text-success"
                  role="status"
                >
                  <span class="sr-only">Loading...</span>
                </div>
              </div>
              <!--listTareas -> Property on Data Layer  -->
              <h5 v-if="listTareas.length == 0">No hay tareas para realizar</h5>

              <ul class="list-group">
                <!-- Data Layer ->tarea: representa mi propiedad tarea / listTareas = Propiedad tipo Lista  -->
                <li
                  v-for="(tarea, index) of listTareas"
                  :key="index"
                  class="list-group-item d-flex justify-content-between"
                >
                  <!-- See below if statement, when tarea.estado equal to true, apply color (class: text-success) -->
                  <span
                    class="cursor"
                    v-bind:class="{ 'text-success': tarea.estado }"
                    v-on:click="editarTarea(tarea, tarea.id)"
                  >
                    <!-- See below if to exchange icons based on tarea.estado -->
                    <i
                      v-bind:class="[
                        tarea.estado ? 'fas fa-check-circle' : 'far fa-circle',
                      ]"
                    ></i>
                  </span>
                  <!-- Constante objeto tarea con propiedades (Nombe ->Que a su ves es la prop tarea & propiedad estado) -->
                  {{ tarea.nombre }}
                  <span
                    class="text-danger cursor"
                    v-on:click="eliminarTarea(tarea.id)"
                  >
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
import axios from "axios";

export default {
  name: "Tarea",
  data() {
    return {
      tarea: "",
      listTareas: [],
      loading: false,
    };
  },
  methods: {
    agregarTarea() {
      const tarea = {
        nombre: this.tarea,
      };
      // this.listTareas.push(tarea);
      this.loading = true;
      axios
        .post("https://localhost:44384/api/Tarea/", tarea)
        .then((response) => {
          console.log(response);
          this.loading = false;
          this.obtenerTareas();
        })
        .catch((error) => {
          console.error(error);
          this.loading = false;
        });

      this.tarea = "";
    },
    eliminarTarea(id) {
      /* this.listTareas.splice(index, 1); */
      this.loading = true;
      axios
        .delete("https://localhost:44384/api/Tarea/" + id)
        .then((response) => {
          console.log(response);
          this.loading = false;
          this.obtenerTareas();
        })
        .catch((error) => {
          console.log(error);
          this.loading = false;
        });
    },
    editarTarea(tarea, id) {
      // this.listTareas[index].estado = !tarea.estado;
      this.loading = true;
      axios
        .put("https://localhost:44384/api/Tarea/" + id, tarea)
        .then((response) => {
          console.log(response);
          this.loading = false;
          this.obtenerTareas();
        })
        .catch((error) => {
          this.loading = false;
          console.log(error);
        });    
          
    },
    obtenerTareas() {
      this.loading = true;
      axios
        .get("https://localhost:44384/api/Tarea")
        .then((response) => {
          console.log(response);
          this.listTareas = response.data;
          this.loading = false;
        })
        .catch((error) => {
          console.log(error);
          this.loading = false;
        });
    },
  },
  created: function () {
    this.obtenerTareas();
  },
};
</script>

<style  scoped>
.cursor {
  cursor: pointer;
}
</style>