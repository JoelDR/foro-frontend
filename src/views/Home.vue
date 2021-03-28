<template>
  <div class="home">
    <div class="container">
      <div class="row text-center">
        <div class="col-md-6 offset-md-3">
          <div class="card">
            <div class="card-body">
              <div class="login-img">
                <!--<img
                  src="https://demos6.softaculous.com/PrestaShop/img/preston-login@2x.png"
                />-->
              </div>
              <div class="login-title">
                <h4>Iniciar Sesión</h4>
              </div>
              <div class="login-form mt-4">
                <form v-on:submit.prevent="login">
                  <div class="form-row">
                    <div class="form-group col-md-12">
                      <input
                        id="email"
                        name="Full Name"
                        placeholder="Correo"
                        class="form-control"
                        type="text"
                        v-model="usuario"
                      />
                    </div>
                    <div class="form-group col-md-12">
                      <input
                        type="pass"
                        class="form-control"
                        id="pass"
                        placeholder="Contraseña"
                        v-model="password"
                      />
                    </div>
                  </div>
                  <div class="form-row">
                    <button type="submit" class="btn btn-success btn-block">
                      Iniciar Sesión
                    </button>
                  </div>
                  <div class="form-row mt-4">
                    <a class="btn btn-info btn-block" href="/register">
                      Registrarse
                    </a>
                  </div>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios';
export default {
  name: "Home",
  components: {
  },
  data: function() {
    return  {
      usuario: "",
      password: "",
      error: false,
      error_msg: "",
      ListaUsuarios: {
        type:Array,
      },
      user: {
        "id_usuario": "",
        "nombre": "",
        "apellido": "",
      },
    }
  },
  mounted: function() {
    let url = "http://localhost:8080/users";
      axios.get(url).then(data => {
        this.ListaUsuarios = data.data;
        console.log(this.ListaUsuarios);
      });
    localStorage.removeItem('user');
  },
  methods: {
    login() {
      const mail = this.usuario;
      const pass = this.password;
      let filtrar = this.ListaUsuarios.filter((u) => (u.email.match(mail) && u.clave.match(pass)));
      if (filtrar.length == 1) {
        this.user.id_usuario = filtrar[0].id_usuario;
        this.user.nombre = filtrar[0].nombres;
        this.user.apellido = filtrar[0].apellidos;
        localStorage.setItem('user', JSON.stringify(this.user));
        this.$router.push('Foro');
      }
    }
  }
};
</script>

<style scoped>
  .login-img img {width:10%; max-height:20%;}
</style>