<template>
    <div class="container-fluid">
        <div class="row" id="head">
            <div class="col-md-2">
                <router-link to="/carte" class="btn btn-default btn-lg">
                    Retour carte
                </router-link>
            </div>
            <div class="col-md-6">
                <h1>Selection variables</h1>
            </div>
            <div class="checkbox col-md-2">
                <label><input type="checkbox" value="" v-model="pourcentage">Pourcentage</label>
            </div>
        </div>

        <div class="row">
            <div class="col-md-8 col-md-offset-2">
                <div class="panel-group" id="accordion" role="tablist" aria-multiselectable="true" v-show="dataset.length > 0">
                    <div class="panel panel-info" v-for="d, i in dataset">
                        <div class="panel-heading" role="tab" :id="'heading'+i">
                            <h4 class="panel-title">
                                <a role="button" data-toggle="collapse" data-parent="#accordion" :href="'#collapse'+i" aria-expanded="true" aria-controls="collapseOne">
                                     {{ d.nom }}
                                </a>
                            </h4>
                        </div>
                        <div :id="'collapse'+i" class="panel-collapse collapse in" role="tabpanel" :aria-labelledby="'heading'+i">
                            <div class="panel-body">
                                <div class="row">
                                    <div class="col-md-12">
                                        <table class="table table-bordered table-responsive text-center">
                                        <tbody>
                                            <tr class="row" v-for="v in d.variables" v-if="!pourcentage">
                                                <td class="col-md-7">
                                                    <p>{{ v.nom }}</p>
                                                </td>
                                                <td class="col-md-4">
                                                    <p>{{ v.codeVar }}</p>
                                                </td>
                                                <td class="col-md-1" v-if="v.id == variable.id">
                                                    <button class="btn btn-danger" v-on:click='changeEtatVariable(v, "masquer")'>
                                                        <span class ="glyphicon glyphicon-flag"></span> Masquer
                                                    </button>
                                                </td>
                                                <td class="col-md-2" v-else>
                                                    <button class="btn btn-info" v-on:click='changeEtatVariable(v, "afficher")'>
                                                        <span class ="glyphicon glyphicon-flag"></span> Afficher
                                                    </button>
                                                </td>
                                            </tr>
                                            <tr class="row" v-for="v in d.variables" v-if="pourcentage">
                                                <td class="col-md-5">
                                                    <p>{{ v.nom }}</p>
                                                </td>
                                                <td class="col-md-3">
                                                    <p>{{ v.codeVar }}</p>
                                                </td>
                                                <td class="col-md-2" v-if="v.id == variable.id">
                                                    <button class="btn btn-danger" v-on:click='changeEtatVariable(v, "masquer")'>
                                                        <span class ="glyphicon glyphicon-flag"></span> Masquer
                                                    </button>
                                                </td>
                                                <td class="col-md-2" v-else>
                                                    <button class="btn btn-info" v-on:click='changeEtatVariable(v, "afficher")'>
                                                        <span class ="glyphicon glyphicon-flag"></span> Afficher
                                                    </button>
                                                </td>
                                                <td class="col-md-2" v-if="v.id == varRefPourcentage.id">
                                                    <button class="btn btn-danger">
                                                        <span class ="glyphicon glyphicon-flag"></span> Référence
                                                    </button>
                                                </td>
                                                <td class="col-md-2" v-else>
                                                    <button class="btn btn-info" v-on:click='changeReferencePourcentage(v)'>
                                                        <span class ="glyphicon glyphicon-flag"></span> Référence
                                                    </button>
                                                </td>
                                            </tr>
                                        </tbody>
                                    </table>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-md-8 col-md-offset-2" v-show="dataset.length <= 0">
                    <p>Pas de dataset enregistrés !</p>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
    import * as DatasetTypes from '../../store/dataset/datasetTypes'
    import * as CarteTypes from '../../store/carte/carteTypes'
    export default{
        name:'dataset',
        data (){
            return{
            }
        },
        components:{

        },
        methods:{
            changeEtatVariable(v, c){
                if (c == "afficher")
                    this.$store.dispatch(CarteTypes.SET_VARIABLE,v);
                else
                    this.$store.dispatch(CarteTypes.REMOVE_VARIABLE,v);
            },
            changeReferencePourcentage(v){
                this.$store.dispatch(CarteTypes.SET_REFERENCE_POURCENTAGE,v);
            }
        },
        computed:{
            dataset: function() {
                return this.$store.getters[DatasetTypes.GET_DATASET_LIST]
            },
            variable: function () {
                return this.$store.getters[CarteTypes.GET_VARIABLE]
            },
            pourcentage: {
                get(){
                    return this.$store.getters[CarteTypes.GET_POURCENTAGE]
                },
                set(p){
                    this.$store.dispatch(CarteTypes.SET_POURCENTAGE,p);
                }
            },
            varRefPourcentage(){
                return this.$store.getters[CarteTypes.GET_REFERENCE_POURCENTAGE]
            }
        },
        mounted() {
            $('.collapse').collapse();
        },
        beforeCreate:function(){
            if(this.dataset = [])
                this.$store.dispatch(DatasetTypes.FETCH_DATASET);
        },
        
    }
</script>
<style>
    #head {
        margin-bottom: 2rem;
    }

    h1 {
        margin: 0;
    }

    td {
        height: 30px;
        line-height: 30px !important;
    }

    .table {
        margin: 0;
    }

    .table-bordered {
        border-left: 0;
        border-right: 0;
    }

    p {
        margin: 0;
    }
</style>