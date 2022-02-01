<template>
    <div>
        <h1 class="display-4 text-center">Listado de tareas</h1>
        <hr>
        <div class="row">
            <div class="col-lg-8 offset-lg-2">
                <div class="card mt-4">
                    <div class="card-body">
                        <div class="input-group">
                            <input type="text" v-model="tarea" @keyup.enter="agregarTarea()"
                                class="form-control form-control-lg" ref="agregartareainput" placeholder="Agregar Tarea">
                            <div class="input-group-append">
                               <button v-on:click = "agregarTarea()"
                                    class="btn btn-success btn-lg">Agregar</button>
                            </div>
                        </div>
                        <br>    
                        <h5 v-if="listTareas.length == 0" >No hay tareas pendientes</h5>
                        <br>
                        <ul class="list-group">
                            <li v-for="(tarea,index) of listTareas" :key = "index" 
                                    class="list-group-item list-group-item d-flex justify-content-between">
                                <span class="cursor" v-bind:class="{'text-success' : tarea.estado }"
                                    v-on:click="modificarEstadoTarea(tarea, tarea.id)">
                                    <i v-bind:class="[!tarea.estado ? 'far fa-circle' : 'far fa-check-circle']"></i>                                    
                                </span>
                                {{tarea.nombre}}
                                <span class="text-danger cursor" v-on:click="eliminarTarea(tarea.id)">
                                    <i class="fas fa-trash"></i>
                                </span>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from "axios"

    export default {
        name: 'Tarea',
        data() {
            return{
                tarea: '',
                listTareas: []
            }
        },
        methods: {
            agregarTarea() {
                    const tarea = {
                        nombre: this.tarea,
                        estado: false
                    }
                    axios.post("https://localhost:44369/api/Tarea",tarea).then(response => {
                        console.log(response);
                        this.tarea = '';               
                        this.obtenerTareas();
                    }).catch(error => {console.error();(error)
                })             

            },
            eliminarTarea(id) {
                    axios.delete("https://localhost:44369/api/Tarea/"+ id).then(response => {
                    console.log(response)
                    this.obtenerTareas();
                }).catch(error => console.error(error))
               
            },
            modificarEstadoTarea(tarea, id){
                 axios.put("https://localhost:44369/api/Tarea/"+ id,tarea).then(response => {
                    console.log(response)
                    this.obtenerTareas();
                }).catch(error => console.error(error))               
            },
            obtenerTareas(){
                axios.get("https://localhost:44369/api/Tarea").then(response => {
                    console.log(response)
                    this.listTareas= response.data;
                })                
            }
        },
        created: function(){
            this.obtenerTareas();
        }
    }
</script>

<style scoped>
.cursor{
    cursor: pointer;
}
</style>