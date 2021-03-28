<template>
    <div>
        <!--
        <div class="alert alert-primary" role="alert" v-for="res in ListaReplies" :key="res.id_respuesta" >
            <p>{{res.mensaje}}</p>
        </div>
        <div>
            <p>HOLA</p>
        </div>-->
        <div class="alert alert-light"  role="alert" v-for="res in ListaReplies" :key="res.id_respuesta">
            <div class="row">
                <div class="col-md-1">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="black" width="48px" height="48px"><path d="M0 0h24v24H0z" fill="none"/><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 3c1.66 0 3 1.34 3 3s-1.34 3-3 3-3-1.34-3-3 1.34-3 3-3zm0 14.2c-2.5 0-4.71-1.28-6-3.22.03-1.99 4-3.08 6-3.08 1.99 0 5.97 1.09 6 3.08-1.29 1.94-3.5 3.22-6 3.22z"/></svg>
                </div>
                <div class="col-md-10">
                    <p>{{res.nick}}</p>
                    <br>
                    <label for=""><strong>{{res.fecha}}</strong></label>
                    <p>{{res.mensaje}}</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Respuestas',
    props: {
        ListaReplies: null,
    },
}
</script>