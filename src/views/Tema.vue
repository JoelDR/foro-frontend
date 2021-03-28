<template>
    <div class="w-75 ml-auto mr-auto mt-4 min-vh-100" style="background-color: #E2E8F0">
        <!-- Toast -->
        <div id="toastRespuesta" style="display: none; position: relative; z-index: 2000;">
        <div class="toast" style="position: absolute; top: 0; right: 0; opacity: 1;">
            <div class="toast-header">
            <strong class="mr-auto">Mensaje</strong>
            <button
                type="button"
                id="btnCerarToastProducto"
                @click="cerrarToast"
                class="ml-2 mb-1 close"
            >
                <span aria-hidden="true">&times;</span>
            </button>
            </div>
            <div class="toast-body"></div>
        </div>
        </div>
        <button type="button" class="btn btn-light btn-sm mb-4" @click="verTemas">Regresar</button>
        <div class="alert alert-light"  role="alert">
            <div class="row">
                <div class="col-md-1">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="black" width="48px" height="48px"><path d="M0 0h24v24H0z" fill="none"/><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 3c1.66 0 3 1.34 3 3s-1.34 3-3 3-3-1.34-3-3 1.34-3 3-3zm0 14.2c-2.5 0-4.71-1.28-6-3.22.03-1.99 4-3.08 6-3.08 1.99 0 5.97 1.09 6 3.08-1.29 1.94-3.5 3.22-6 3.22z"/></svg>
                </div>
                <div class="col-md-10">
                    <small>{{tema.nick}}</small>
                    <br>
                    <label for=""><strong>{{tema.titulo}}</strong></label>
                    <p>{{tema.mensaje}}</p>
                </div>
            </div>
            <!--
            <form>
                <div class="form-group">
                    <small>{{tema.fecha}}</small>
                    <label for="formGroupExampleInput" class="w-100 font-weight-bold">TEMA:</label>
                    <label for="formGroupExampleInput" class="w-100">{{tema.titulo}}</label>
                </div>
                <div class="form-group">
                    <label for="formGroupExampleInput2" class="font-weight-bold">ASUNTO:</label>
                    <label for="formGroupExampleInput2" class="w-100">{{tema.mensaje}}</label>
                </div>
            </form>-->
        </div>
        <form class="">
            <div class="form-group">
                <textarea class="form-control" placeholder="Escriba aquí su comentario..." v-model="reply.mensaje" rows="5" ></textarea>
            </div>
            <button type="button" class="btn btn-primary" @click="crearRespuesta">Comentar</button>
            <div class="alert alert-danger mt-4" role="alert" v-if="error">
                Ingrese un comentario
            </div>  
        </form>
        <label for="respuestas" class="mt-4"><strong>Respuestas {{tema.respuesta}} </strong></label>
        <Respuestas :ListaReplies="ListaReply"/>
    </div>
</template>

<script>
import Respuestas from '../views/Respuestas';
import axios from 'axios';

export default {
    name: 'Tema',
    props: {
        tema: null,
        topic: {
            type: Array,
        },
        ListaReply: null,
    },
    data () {
        return {
            reply: {
                "id_tema": "",
                "id_usuario": "",
                "mensaje": "",
            },
            error: false,
        }
    },
    components: {
        Respuestas,
    },
    methods: {
        verTemas() {
            this.$parent.verTema(this.tema);
            this.topic=null;
            this.error=false;
        },
        crearRespuesta() {
            if(this.reply.mensaje !== ""){
                this.reply.id_usuario = JSON.parse(localStorage.getItem('user')).id_usuario;
                this.reply.id_tema = this.tema.id_tema;
                console.log(this.reply);
                this.error = false;

                let url = "http://localhost:8080/reply";
                axios.post(url, this.reply).then(data => {
                    console.log(data);
                });
                this.abrirToast();
                this.limpiarDatos();
            } else {
                this.error = true;
            }
        },
        cerrarToast() {
            const toast = document.getElementById("toastRespuesta");
            toast.style.display = "none";
        },
        abrirToast() {
            const toast = document.getElementById("toastRespuesta");
            const toastBody = toast.getElementsByClassName("toast-body");
            toastBody[0].innerHTML = "¡Comentario registrado exitosamente!";
            toast.style.display = "block";
        },
        limpiarDatos() {
            this.reply.id_usuario= "",
            this.reply.id_tema="",
            this.reply.mensaje=""
        }
    }
}
</script>