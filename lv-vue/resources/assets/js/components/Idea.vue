<template>
    <div>
        <h2 class="text-center">Captura tus ideas</h2>
        <div class="card card-body bg-light">
            <h4>¿En que estas pensando?</h4>
            <form v-on:submit.prevent="createIdea">
                <!-- <div class="input-group input-group-sm mb-3">
                    <input type="text" class="form-control input-sm" maxlength="256">
                    <span class="input-group-text">
                        
                    </span>
                </div> -->
                <div class="input-group input-group-sm mb-3">
                    <input type="text" class="form-control input-sm" v-model="nuevaIdea" maxlength="256">
                    <span class="input-group-text">
                        <button type="submit" class="btn btn-primary btn-sm">
                            Agregar
                        </button>
                    </span>
                </div>
            </form>
            <hr>
            <ul class="list-unstyled">
                <li v-for="idea in ideas">
                    <p>
                        <small class="text-muted">
                            <em>{{ since(idea.created_at) }}</em>
                        </small>
                        {{idea.descripcion}}
                    </p>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import moment from 'moment';
    import toastr from 'toastr';

    moment.locale('es');

    export default {
        mode: 'production',

        data() {
            return {
                ideas: [],
                nuevaIdea: ''
            }
        },
        created: function(){
            this.getIdeas();
        },
        methods:{
            since: function(d){
                return moment(d).fromNow();
            },
            getIdeas: function(){
                var urlIdeas = 'mis-ideas';
                axios.get(urlIdeas).then(response => {
                    this.ideas = response.data
                });
            },
            createIdea: function(){
                var url = 'guardar-idea';
                axios.post(url, {
                    descripcion: this.nuevaIdea
                }).then(response =>{
                    this.getIdeas();
                    this.nuevaIdea = '';
                    toastr.success('Nueva Idea Registrada');
                }).catch(error => {
                    toastr.error('Error');
                })
            }
        }
        
    }
</script>
