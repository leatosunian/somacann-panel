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
                                            <b>{{clientDat.name}}</b>
                                        </h1>
                                    </div>
                                </div> <!-- / .row -->
                            </div>
                        </div>

                        <div class="row">
                            <div class="card-header" style="padding-left:22px">
                                <!-- Title -->
                                <h4 class="mb-0 card-header-title" style="font-size:17px">
                                  <b>Datos del cliente</b>
                                </h4>
                            </div>
                            <div class="col-12 col-md-6">
                                <div class="card">
                                <div class="card-body" style="padding: 0.8rem !important;">
                                    <!-- List group -->
                                    <div class="list-group list-group-flush my-n3">
                                    
                                    <div class="list-group-item">
                                        <div class="row align-items-center">
                                        <div class="col">
                                            <!-- Title -->
                                            <h5 class="mb-0">Nombre</h5>
                                        </div>
                                        <div class="col-auto">
                                            <!-- Text -->
                                            <small class="text-muted" >
                                                {{clientDat.name}}
                                            </small>
                                        </div>
                                        </div>
                                        <!-- / .row -->
                                    </div>
                                    <div class="list-group-item">
                                        <div class="row align-items-center">
                                        <div class="col">
                                            <!-- Title -->
                                            <h5 class="mb-0">Correo</h5>
                                        </div>
                                        <div class="col-auto">
                                            <!-- Text -->
                                            <small class="text-muted" >
                                            {{clientDat.email}}
                                            </small>
                                        </div>
                                        </div>
                                        <!-- / .row -->
                                    </div>
                                    <div class="list-group-item">
                                        <div class="row align-items-center">
                                            <div class="col">
                                                <!-- Title -->
                                                <h5 class="mb-0">Fecha de registro</h5>
                                            </div>
                                            <div class="col-auto">
                                                <!-- Link -->
                                                <span class="small text-muted">
                                                {{convertDate(clientDat.createdAt)}}
                                                </span>
                                            </div>
                                        </div>
                                        <!-- / .row -->
                                    </div>
                                    </div>
                                </div>
                                </div>
                            </div>
                            <div class="col-12 col-md-6">
                                <div class="card">
                                    <div class="card-body" style="padding: 0.8rem !important;">
                                        <!-- List group -->
                                        <div class="list-group list-group-flush my-n3">
                                            <div class="list-group-item">
                                                <div class="row align-items-center">
                                                <div class="col">
                                                    <!-- Title -->
                                                    <h5 class="mb-0">Estado</h5>
                                                </div>
                                                <div class="col-auto">
                                                    <!-- Text -->
                                                    <small class="text-muted" v-if="clientDat.enabled == true">
                                                        Habilitado
                                                    </small>
                                                    <small class="text-muted" v-if="clientDat.enabled == false">
                                                        Deshabilitado
                                                    </small>
                                                </div>
                                                </div>
                                                <!-- / .row -->
                                            </div>
                        
                                            <div class="list-group-item">
                                                <div class="row align-items-center">
                                                <div class="col">
                                                    <!-- Title -->
                                                    <h5 class="mb-0">Compras</h5>
                                                </div>
                                                <div class="col-auto">
                                                    <!-- Text -->
                                                    <small class="text-muted" >
                                                        {{orders.length}}
                                                    </small>
                                                </div>
                                                </div>
                                                <!-- / .row -->
                                            </div>
                        
                                            <div class="list-group-item">
                                                <div class="row align-items-center">
                                                <div class="col">
                                                    <!-- Title -->
                                                    <h5 class="mb-0">Total en compras</h5>
                                                </div>
                                                <div class="col-auto">
                                                    <!-- Link -->
                                                    <span class="small text-muted">
                                                    {{ convertCurrency(earnings) }}
                                                    </span>
                                                </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <!-- SHIPPING DATA -->
                        <div class="row">
                            <div class="card-header" style="padding-left:22px">
                                <!-- Title -->
                                <h4 class="mb-0 card-header-title" style="font-size:17px">
                                <b>Datos de envío</b>
                                </h4>
                            </div>
                            <template v-for="address in addresses" >
                                <div class="col-12 col-md-6">
                                    <div class="card">
                                        <div class="card-body" style="padding: 0.8rem !important;">
                                        <!-- List group -->
                                        <div class="list-group list-group-flush my-n3">
                                            <div class="list-group-item">
                                            <div class="row align-items-center">
                                                <div class="col">
                                                <!-- Title -->
                                                <h5 class="mb-0">Nombre del destinatario</h5>
                                                </div>
                                                <div class="col-auto">
                                                <!-- Time -->
                                                <time
                                                    class="small text-muted"
                                                    datetime="1988-10-24"
                                                >
                                                    {{address.surname}}, {{address.name}}
                                                </time>
                    
                                                </div>
                                            </div>
                                            <!-- / .row -->
                                            </div>
                                            <div class="list-group-item">
                                            <div class="row align-items-center">
                                                <div class="col">
                                                <!-- Title -->
                                                <h5 class="mb-0">N° de DNI</h5>
                                                </div>
                                                <div class="col-auto">
                                                <!-- Time -->
                                                <time
                                                    class="small text-muted"
                                                    datetime="2018-10-28"
                                                >
                                                    {{ address.dni }}
                                                </time>
                                                </div>
                                            </div>
                                            <!-- / .row -->
                                            </div>
                                            <div class="list-group-item">
                                            <div class="row align-items-center">
                                                <div class="col">
                                                <!-- Title -->
                                                <h5 class="mb-0">Teléfono</h5>
                                                </div>
                                                <div class="col-auto">
                                                <!-- Text -->
                                                <small class="text-muted" >
                                                    {{ address.phone }}
                                                </small>
                                                </div>
                                            </div>
                                            <!-- / .row -->
                                            </div>
                                            <div class="list-group-item">
                                                <div class="row align-items-center">
                                                <div class="col">
                                                    <!-- Title -->
                                                    <h5 class="mb-0">Código Postal</h5>
                                                </div>
                                                <div class="col-auto">
                                                    <!-- Text -->
                                                    <small class="text-muted" >
                                                    {{ address.code }}
                                                    </small>
                                                </div>
                                                </div>
                                                <!-- / .row -->
                                            </div>
                                        </div>
                                        </div>
                                    </div>
                                </div>
                                <div class="col-12 col-md-6">
                                    <div class="card">
                                        <div class="card-body" style="padding: 0.8rem !important;">
                                        <!-- List group -->
                                        <div class="list-group list-group-flush my-n3">
                                            <div class="list-group-item">
                                            <div class="row align-items-center">
                                                <div class="col">
                                                <!-- Title -->
                                                <h5 class="mb-0">Ciudad</h5>
                                                </div>
                                                <div class="col-auto">
                                                <!-- Time -->
                                                <time
                                                    class="small text-muted"
                                                    datetime="1988-10-24"
                                                >
                                                    {{ address.city }}
                                                </time>
                                                </div>
                                            </div>
                                            <!-- / .row -->
                                            </div>
                                            <div class="list-group-item">
                                            <div class="row align-items-center">
                                                <div class="col">
                                                <!-- Title -->
                                                <h5 class="mb-0">Provincia</h5>
                                                </div>
                                                <div class="col-auto">
                                                <!-- Time -->
                                                <time
                                                    class="small text-muted"
                                                    datetime="2018-10-28"
                                                >
                                                    {{ address.province }}
                                                </time>
                                                </div>
                                            </div>
                                            <!-- / .row -->
                                            </div>
                    
                                            <div class="list-group-item">
                                            <div class="row align-items-center">
                                                <div class="col">
                                                <!-- Title -->
                                                <h5 class="mb-0">Domicilio</h5>
                                                </div>
                                                <div class="col-auto">
                                                <!-- Link -->
                                                <span class="small text-muted">
                                                    {{ address.address }}
                                                </span>
                                                </div>
                                            </div>
                                            <!-- / .row -->
                                            </div>
    
                                            <div class="list-group-item">
                                                <div class="row align-items-center">
                                                <div class="col">
                                                    <!-- Title -->
                                                    <h5 class="mb-0">Piso</h5>
                                                </div>
                                                <div class="col-auto">
                                                    <!-- Link -->
                                                    <span class="small text-muted" v-if="address.floor">
                                                    {{ address.floor }}
                                                    </span>
                                                    <span class="small text-muted" v-if="!address.floor">
                                                        No especificado
                                                    </span>
                                                </div>
                                                </div>
                                                <!-- / .row -->
                                            </div>
                                        </div>
                                        </div>
                                    </div>
                                   
                                </div> 
                                <div class="" style="border:1px solid #74747462; width:70%; margin:0 auto; margin-bottom:14px;">

                                </div>                           
                            </template>
                        </div>
                        
                        <!-- ORDERS -->
                        <div class="card">
                            <div class="card-header">
                              <!-- Title -->
                              <h4 class="mb-0 card-header-title">
                                <b style="font-size:17px">Pedidos</b>
                              </h4>
                            </div>
                            <div  class="table-responsive">
                                <table class="table table-sm table-nowrap card-table">
                                    <thead>
                                    <tr>
                                        <th>Número de pedido</th>
                                        <th>Fecha</th>
                                        <th>Total</th>
                                        <th> </th>
                                    </tr>
                                    </thead>
                                    <tbody class="fs-base" v-for="order in orders"  >
                                        <tr>
                                            <td>
                                            {{ order.orderNumber }}
                                            </td>
                                            <td>
                                            {{ order.day }}/{{ order.month }}/{{ order.year }}
                                            </td>
                                            <td>
                                            {{ convertCurrency(order.total) }}
                                            </td>
                                            <td>
                                                <router-link :to="{name: 'order',params: {id: order._id}}" class="btn btn-sm" style="background: linear-gradient(120.6deg, rgb(179 134 149) -18.3%, rgb(67, 54, 74) 16.4%, rgb(47, 48, 67) 68.2%, rgb(69 41 90) 99.1%); color:#f9f9f9 !important;">
                                                    <a style="color:#f9f9f9 !important;"  >Datos</a>
                                                </router-link>
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
import Sidebar from '@/components/Sidebar.vue';
import moment from "moment";
import currency_formatter from "currency-formatter";

export default {
    name: "ClientDetail",
    components: {
        Sidebar
    },
    data(){
        return {
            orders: [],
            clientDat: {},
            addresses: [],
            earnings: 0
        }
    },
    beforeMount(){
        this.getClient()
    },
    methods: {
        convertCurrency(number) {
            return currency_formatter.format(number, { code: "ARS" });
        },
        convertDate(item) {
            return moment(item).format("DD/MM/YYYY");
        },

        getClient() {
            const token = localStorage.getItem('token')
            axios.get(this.$url+'/clients/'+this.$route.params.id,{
                headers:{
                        'Content-Type': 'application/json',
                        'Authorization': `Bearer ${token}`
                }
            }).then((result)=>{
                const {data} = result

                if(data.clientData){
                    this.clientDat = data.clientData
                }
                if(data.clientAddresses){
                    this.addresses = data.clientAddresses
                }
                if(data.clientPurchases){
                    this.orders = data.clientPurchases
                }
                let totalEarnings = 0
                let datos = this.orders.map((sale) => {
                    const saleData = Object.entries(sale)
                    const saleTotal = saleData[1]
                    const total = saleTotal[1]
                    totalEarnings = totalEarnings + total                  
                })
                this.earnings = totalEarnings
            });
        },
    }
}
</script>