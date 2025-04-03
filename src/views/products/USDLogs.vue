<template>
    <div>
        <template>
            <div class="vld-parent" style="z-index: 999999 !important;">
                <loading :active.sync="isLoading" :can-cancel="false" :is-full-page=true :opacity=0.61 />
            </div>
        </template>
        <Sidebar/>
        <div class="main-content contentPadding">
            <div class="container-fluid">
                <div class="row justify-content-center">
                    <div class="col-12 col-lg-10 col-xl-8">
                        <!-- Header -->
                        <div class="header mt-md-5">
                            <div class="header-body">
                                <div class="row align-items-center">

                                    <div class="col">
                                        <!-- Pretitle -->
                                        <h6 class="header-pretitle">
                                        Configuraciones
                                        </h6>
                    
                                        <!-- Title -->
                                        <h1 class="header-title">
                                        Historial de cotizaciones
                                        </h1>
                                    </div>

                                </div> <!-- / .row -->
                                
                            </div>
                        </div>
                        <div class="card">

                            <div class="table-responsive">
                                <table class="table table-sm table-nowrap card-table">
                                <thead>
                                    <tr>
                                        <th>Fecha de edición</th>
                                        <th>Cotización</th>
                                    </tr>
                                </thead>
                                <tbody class="fs-base" v-if="USDLogs.length >= 1">
                                    <tr v-for="item in USDLogs">
                                        <td>
                                            <a>{{dateConverter(item.createdAt)}}</a>
                                        </td>
                                        <td>
                                            <a>{{priceConverter(item.value)}}</a>
                                        </td>
                                    </tr>
                                    
                                </tbody>
                                <tbody v-if="USDLogs.length == 0">
                                    <tr>
                                        <td colspan="4">
                                            <div class="row justify-content-center">
                                                    <div class="my-5 col-12 col-md-6 col-xl-6">
                                                        
                                                        <div class="text-center">

                                                            <!-- Heading -->
                                                            <h1 class="mb-3 display-4">
                                                                No se encontraron nuevos pedidos
                                                            </h1>

                                                            <!-- Subheading -->
                                                            <p class="mb-4 text-muted">
                                                                No hay datos que mostrar.
                                                            </p>
                                                        </div>

                                                    </div>
                                                </div>  
                                        </td>
                                    </tr>
                                </tbody>
                                </table>
                            </div>
                        </div>                            
                    </div>
                </div>
            </div> <!-- / .row -->
        </div>
    
        
    </div>
</template>



<script>
import Sidebar from '@/components/Sidebar.vue'
import axios from 'axios'
import Loading from 'vue-loading-overlay';
import 'vue-loading-overlay/dist/vue-loading.css';
import moment from 'moment'
import currencyFormatter from 'currency-formatter'

export default {
    name: "USDLogs",
    components: {
        Sidebar,
        Loading
    },
    data(){
        return {
            USDLogs: {},
        }
    },
    beforeMount(){
        this.getUSDLogs()
    },
    mounted(){
        window.scrollTo(0, 0)
    },
    methods: {    
        dateConverter(date){
            return moment(date).format('DD-MM-YYYY')
        },
        priceConverter(price){
            return currencyFormatter.format(price, { code: 'ARS' });
        },
        getUSDLogs(){
            const user = localStorage.getItem('user_id')
            axios.get(`${this.$url}/config/getusdlogs`, {
                headers: {
                    "Content-Type": 'application/json',
                    "Authorization" : `Bearer ${this.$token}`
            }
            }).then((response) => {
                this.USDLogs = response.data
            }).catch( error => {
                console.log(error)
            })
        },

    }
}
</script>