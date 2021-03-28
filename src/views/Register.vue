<template>
    <div class="container register-form">
            <div class="form">
                <div class="note">
                    <p>Formulario de Registro</p>
                </div>
                <form v-on:submit.prevent="createUser" class="form-content">
                    <div class="row">
                        <div class="col-md-6">
                            <div class="form-group">
                                <input type="text" class="form-control" placeholder="Nombres *" v-model="usuario.nombres"/>
                            </div>
                            <div class="form-group">
                                <input type="text" class="form-control" placeholder="Email*" v-model="usuario.email"/>
                            </div>
                            <div class="form-group">
                                <input type="text" class="form-control" placeholder="Nickname *" v-model="usuario.nick"/>
                            </div>
                        </div>
                        <div class="col-md-6">
                            <div class="form-group">
                                <input type="text" class="form-control" placeholder="Apellidos *" v-model="usuario.apellidos"/>
                            </div>
                            <div class="form-group">
                                <input type="password" class="form-control" id="pass" placeholder="Contraseña *" v-model="usuario.clave"/>
                            </div>
                        </div>
                    </div>
                    <button type="submit" class="btnSubmit" @click="createUser">Registrarse</button>
                </form>
            </div>
        </div>
</template>

<script>
import axios from 'axios';
export default {
    name: "Register",
    data: function() {
        return {
            usuario: {
                "nombres": "",
                "apellidos": "",
                "email": "",
                "nick": "",
                "clave": "",
            }
        }
    },
    methods: {
        createUser() {
            let url = "http://localhost:8080/users";
            axios.post(url, this.usuario).then(data => {
            console.log(data);
            });
        }
    }
}
</script>

<style scoped>
    .note
{
    text-align: center;
    height: 80px;
    background: -webkit-linear-gradient(left, #0072ff, #8811c5);
    color: #fff;
    font-weight: bold;
    line-height: 80px;
}
.form-content
{
    padding: 5%;
    border: 1px solid #ced4da;
    margin-bottom: 2%;
}
.form-control{
    border-radius:1.5rem;
}
.btnSubmit
{
    border:none;
    border-radius:1.5rem;
    padding: 1%;
    width: 20%;
    cursor: pointer;
    background: #0062cc;
    color: #fff;
}
</style>