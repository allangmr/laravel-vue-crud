<template>
    <div class="card">
        <div class="card-header">Publicado en {{pensamiento.created_at}} - Ultima actualizacion {{pensamiento.updated_at}} </div>

        <div class="card-body">
            <input  v-if="editMode"  type="text" class="form-control" v-model="pensamiento.description">
            <p v-else>{{pensamiento.description}}</p>

        </div>
        <div class="card-footer">
            <button class="btn btn-success" v-if="editMode" v-on:click="onClickUpdate()"> Guardar Cambios  </button>
            <button class="btn btn-default" v-else v-on:click="onClickEdit()"> Editar </button>
            <button class="btn btn-danger" v-on:click="onClickDelete()"> Eliminar </button>
        </div>
    </div>
</template>

<script>
    export default {
        props : ['pensamiento'],
        data(){
            return{
                editMode: false
            }
        },
        mounted() {
            console.log('PensamientoComponent mounted.')
        },
        methods:
            {
                onClickDelete(){
                    axios.delete(`pensamiento/${this.pensamiento.id}`).then(() =>{
                        this.$emit('delete');

                    });
                    this.$emit('delete');
                },
                onClickEdit(){
                    this.editMode = true;
                },
                onClickUpdate(){
                    const params = {
                        description: this.pensamiento.description
                    };
                    axios.put(`pensamiento/${this.pensamiento.id}`, params).then((response) =>{
                        this.editMode = false;
                        const pensamiento = response.data;
                        this.$emit('update', pensamiento);

                    });

                },
            }
    }
</script>
