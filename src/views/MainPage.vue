<template>
    <div>
        <NavBar/>
        <div class="w-75 m-auto"  v-show="!form">
            <h1 class="text-start">Comunidad</h1>
            <button type="submit" class="btn btn-primary" @click="createTopic">Nuevo Tema</button>
            <form action="">
                <div class="mb-3">
                    <label for="exampleFormControlInput1" class="form-label">Tema</label>
                    <input type="text" class="form-control" v-model="tema.titulo">
                </div>
                <div class="mb-3">
                    <label for="exampleFormControlTextarea1" class="form-label">Mensaje</label>
                    <textarea class="form-control"  v-model="tema.mensaje" rows="5"></textarea>
                </div>
            </form>
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
            </table>
        </div>
        <Tema v-show="form" :tema="temaObject"/>
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
            tema: {
                "id_usuario": "",
                "titulo": "",
                "mensaje": "",
                "respuesta": 0,
            },
            user: null,
            id_tema: 0,
            temaObject: null,
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
            this.tema.id_usuario = JSON.parse(localStorage.getItem('user')).id_usuario;
            let url = "http://localhost:8080/tema";
            axios.post(url, this.tema).then(data => {
                console.log(data);
            })
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
            }else {
                this.id = 0;
                this.form = !this.form;
            }
        }

    }
}
</script>