<template>
    <div class="w-75 m-auto mt-4">
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
        </form>
        <button class="btn btn-primary" @click="verTemas">Volver</button>
        <form>
            <div class="form-group">
                <input type="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">
                <small id="emailHelp" class="form-text text-muted">Escriba su comentario...</small>
            </div>
            <button type="submit" class="btn btn-primary">Comentar</button>
        </form>
    </div>
</template>

<script>
export default {
    name: 'Tema',
    props: {
        tema: null,
        topic: {
            type: Array,
        }
    },
    methods: {
        verTemas() {
            this.$parent.verTema(this.tema);
            this.topic=null;
        }
    }
}
</script>