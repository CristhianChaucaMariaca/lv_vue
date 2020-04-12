<template>
    <div>
        <h2 class="text-center">Captura tus ideas</h2>
        <div class="well">
            <h4>En que estas pensano</h4>
            <form v-on:submit.prevent="createIdea">
                <div class="input-group">
                    <input type="text" v-model="newIdea" name="" id="" class="form-control form-control-sm" maxlength="256">
                    <span class="input-group-btn">
                        <button class="btn btn-primary btn-sm">Abregar</button>
                    </span>
                </div>
            </form>
            <hr>
            <ul class="list-unstyled">
                <li v-for="idea in ideas">
                    <p>
                        <small class="text-muted">
                            <em>{{since(idea.created_at)}}</em>
                        </small>
                        {{idea.description}}
                    </p>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>

    // Importamos la libreria de forma local para usar solo en este componente
    import axios from 'axios'   // Libreria para usar ajax
    import toastr from 'toastr' // Libreria para alertas
    import moment from 'moment' //Libreria para formatear la fecha de mejor forma

    moment.locale('es'); //Formateamos la moment para que muestre la fecha en español

    export default {
        data(){
            return{
                ideas: [

                ],
                newIdea:''
            }
        },
        created:function () {
            this.getIdeas();
        }
        ,
        methods:{
            since:function(d){
                return moment(d).fromNow();//Desde este momento saca la cuenta respecto la fecha que mandamos
            },
            getIdeas: function(){
                var urlIdeas = 'mis-ideas';
                axios.get(urlIdeas).then(response =>{
                    this.ideas = response.data
                });
            },
            createIdea: function(){
                var url='guardar-idea';
                axios.post(url,{
                    description: this.newIdea
                }).then(response => {
                    this.getIdeas();
                    this.newIdea = '';
                    toastr.success('Nueva idea registrada');
                }).catch(error=>{
                    toastr.error('Error');
                });
            }
        }
    }
</script>
