<template>
    <div style="background-color: #E2E8F0">
        <NavBar/>
        <div class="w-75 m-auto"  v-show="!form">
            <h1 class="text-start">Comunidad</h1>
            <!--<button type="submit" class="btn btn-primary" @click="createTopic">Nuevo Tema</button>
            <form action="">
                <div class="mb-3">
                    <label for="exampleFormControlInput1" class="form-label">Tema</label>
                    <input type="text" class="form-control" v-model="tema.titulo">
                </div>
                <div class="mb-3">
                    <label for="exampleFormControlTextarea1" class="form-label">Mensaje</label>
                    <textarea class="form-control"  v-model="tema.mensaje" rows="5"></textarea>
                </div>
            </form>-->
            <div class="alert justify-content-between mt-4 mb-4 bg-white pt-3 pb-3" role="alert">
                <div class="row justify-content-between">
                    <div class="col-md-4">
                    <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal" v-on:click="error=false">
                        <strong>+ Nueva discusión</strong>
                    </button>
                </div>
                <div class="col-md-4">
                    <input type="text" class="form-control" aria-describedby="emailHelp" placeholder="Buscar tema">
                </div>
                </div>
            </div>
            <div class="alert alert-light"  role="alert" v-for="tema in ListaTemas" :key="tema.id_tema" v-on:click="verTema(tema.id_tema)">
                <div class="row justify-content-between">
                    <div>
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="black" width="48px" height="48px"><path d="M0 0h24v24H0z" fill="none"/><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 3c1.66 0 3 1.34 3 3s-1.34 3-3 3-3-1.34-3-3 1.34-3 3-3zm0 14.2c-2.5 0-4.71-1.28-6-3.22.03-1.99 4-3.08 6-3.08 1.99 0 5.97 1.09 6 3.08-1.29 1.94-3.5 3.22-6 3.22z"/></svg>
                        <label for=""><strong>{{tema.titulo}}</strong></label>
                    </div>
                    <div class="p-2">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="black" width="24px" height="24px"><path d="M0 0h24v24H0z" fill="none"/><path d="M21 6h-2v9H6v2c0 .55.45 1 1 1h11l4 4V7c0-.55-.45-1-1-1zm-4 6V3c0-.55-.45-1-1-1H3c-.55 0-1 .45-1 1v14l4-4h10c.55 0 1-.45 1-1z"/></svg>
                        <label for=""> Respuestas: {{tema.respuesta}}</label>
                    </div>
                </div>
                <p>{{tema.mensaje}}</p>
                <form class="form-group row"></form>
                <small>Autor: {{tema.nick}}</small>
                <br>
                <small>Fecha: {{tema.fecha}}</small>
            </div>
            <!--
            <table class="table table-hover">
                <thead>
                    <tr>
                        <th colspan="2">Tema</th>
                        <th scope="col">Autor</th>
                        <th scope="col">Fecha</th>
                        <th scope="col">Respuesta</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="tema in ListaTemas" :key="tema.id_tema" v-on:click="verTema(tema.id_tema)">
                        <th colspan="2">{{tema.titulo}}</th>
                        <td>{{tema.nick}}</td>
                        <td>{{tema.fecha}}</td>
                        <td>{{tema.respuesta}}</td>
                    </tr>
                </tbody>
            </table>-->
        </div>
        <Tema v-show="form" :tema="temaObject" :ListaReply="ListaRespuestas"/>
        <!--Modal-->
        <!-- Modal -->
        <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog modal-lg">
                <div class="modal-content">
                <div class="modal-header bg-primary">
                    <h5 class="modal-title text-light" id="exampleModalLabel">Nueva discusión</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span class="text-light" aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div class="modal-body">
                    <form>
                        <div class="form-group">
                            <label for="recipient-name" class="col-form-label">Tema</label>
                            <input type="text" class="form-control" id="recipient-name" v-model="tema.titulo">
                        </div>
                        <div class="form-group">
                            <label for="message-text" class="col-form-label">Mensaje</label>
                            <textarea class="form-control" id="message-text" v-model="tema.mensaje" rows="5"></textarea>
                        </div>
                    </form>
                </div>
                <div class="alert alert-danger" role="alert" v-if="error">
                    Ingrese los datos correctamente
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-dismiss="modal">Cerrar</button>
                    <button type="submit" class="btn btn-primary" @click="createTopic">Postear</button>
                </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import NavBar from '@/components/NavBar.vue';
import Tema from '../views/Tema.vue'
export default {
    name: "MainPage",
    components: {
        NavBar,
        Tema,
    },
    data() {
        return {
            form: false,
            ListaTemas: null,
            error: false,
            tema: {
                "id_usuario": "",
                "titulo": "",
                "mensaje": "",
                "respuesta": 0,
            },
            user: null,
            id_tema: 0,
            temaObject: null,
            ListaRespuestas: null,
        }
    },
    mounted: function() {
        let url = "http://localhost:8080/tema";
        axios.get(url).then(data => {
            this.ListaTemas = data.data;
        });
    },
    methods: {
        createTopic() {
            if(this.tema.titulo !==  "" && this.tema.mensaje !== "") {
                this.tema.id_usuario = JSON.parse(localStorage.getItem('user')).id_usuario;
                let url = "http://localhost:8080/tema";
                axios.post(url, this.tema).then(data => {
                    console.log(data);
                });
                this.error = false;
            } else {
                this.error = true;
            }
        },
        verTema(id){
            console.log(id);
            if(!this.form) {
                this.id_tema = id;
                this.form = !this.form;
                let url = "http://localhost:8080/tema/";
                axios.get(url+id).then(data => {
                    this.temaObject = data.data[0];
                    console.log(this.temaObject);
                });
                let url2 = "http://localhost:8080/reply/";
                axios.get(url2+id).then(data => {
                    this.ListaRespuestas = data.data;
                    console.log(this.ListaRespuestas);
                });
            }else {
                this.id = 0;
                this.form = !this.form;
            }
        }

    }
}
</script>