<template>
    <div>
        <Sidebar />
        <div class="main-content contentPadding">
            <div class="container-fluid ">
                <div class="row justify-content-center">
                    <div class="col-12 col-lg-10 col-xl-8">

                        <!-- Header -->
                        <div class="header mt-md-5">
                            <div class="header-body">
                                <div class="row align-items-center">
                                    <div class="col">
                                        <!-- Pretitle -->
                                        <h6 class="header-pretitle">
                                            clientes 
                                        </h6>
                                        <!-- Title -->
                                        <h1 class="header-title">
                                            <b>Todos mis clientes</b>
                                        </h1>
                                    </div>
                                </div> <!-- / .row -->
                            </div>
                        </div>

                        <div class="mb-5 row" style="height: 80px">
                            <div class="card-header">

                                <!-- Title -->
                                <h4 class="mb-0 card-header-title">
                                    <b>Buscar cliente</b>
                                </h4>

                            </div>
                            <div class="input-group input-group-rounded input-group-merge input-group-reverse" >
                                <input v-model="searchFilter" v-on:keyup="searchClient()" class="form-control" type="text" placeholder="Nombre del cliente" aria-label="Search" style="border-right:1px solid #95aac9; border-top:1px solid #95aac9; border-bottom:1px solid #95aac9">
                                <div class="input-group-text" style="border-left:1px solid #95aac9; border-top:1px solid #95aac9; border-bottom:1px solid #95aac9">
                                    <span class="fe fe-search"></span>
                                </div>
                            </div>
                        </div>

                        <div class="card">
                            <div class="table-responsive">
                                <table class="table table-sm table-nowrap card-table">
                                    <thead>
                                        <tr>
                                            <th>Nombre</th>
                                            <th>Correo</th>
                                            <th>Estado</th>
                                            <th></th>
                                        </tr>
                                    </thead>

                                    <tbody class="fs-base" v-if="clients.length >= 1">
                                        <tr v-for="item in clients">
                                            <td>
                                                <a>{{item.name}}</a>
                                            </td>
                                            <td>
                                                <a>{{item.email}}</a>
                                            </td>
                                            <td v-if="item.enabled === true">
                                                Habilitado
                                            </td>

                                            <td v-if="item.enabled === false">
                                                Deshabilitado
                                            </td>

                                            <td>
                                                <router-link :to="{name: 'client',params: {id: item._id}}" class="btn btn-sm" style="background: linear-gradient(120.6deg, rgb(179 134 149) -18.3%, rgb(67, 54, 74) 16.4%, rgb(47, 48, 67) 68.2%, rgb(69 41 90) 99.1%); color:#f9f9f9 !important;">
                                                    <a style="color:#f9f9f9 !important;"  >Datos</a>
                                                </router-link>
                                            </td>
                                        </tr>
                                    </tbody>

                                    <tbody v-if="clients.length == 0">
                                        <tr>
                                            <td colspan="4">
                                                <div class="row justify-content-center">
                                                    <div class="my-5 col-12 col-md-6 col-xl-6">
                                                        <div class="text-center">
                                                            <h1 class="mb-3 display-4">
                                                                No se encontraron registros
                                                            </h1>

                                                            <p class="mb-4 text-muted">
                                                                No hay datos que mostrar
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
                </div> <!-- / .row -->
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import Sidebar from '@/components/Sidebar.vue'

export default {
    name: "ClientsList",
    components: {
        Sidebar
    },
    data(){
        return {
            clients: [],
            searchFilter: ''
        }
    },
    beforeMount(){
        this.getClients()
    },
    methods: {
        getClients() {
            const token = localStorage.getItem('token')
            axios.get(this.$url+'/clients/getall/',{
                headers:{
                        'Content-Type': 'application/json',
                        'Authorization': `Bearer ${token}`
                }
            }).then((result)=>{
                this.clients = result.data;
            });
        },
        searchClient() {
            const filter = this.searchFilter
            this.clients = this.clients.filter(item => item.name.toLowerCase().includes(filter.toLowerCase()))
            if(filter == ''){
                return this.getClients()
            }
        }
    }
}
</script>