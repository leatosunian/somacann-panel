<template>
  <div>
    <template>
      <div class="vld-parent" style="z-index: 999999 !important;">
          <loading :active.sync="isLoading" :can-cancel="false" :is-full-page=true  :opacity=0.61 />
      </div>
    </template>
    <Sidebar/>
    <div class="main-content ">

      <div class="container-fluid">

        <div class="row marginTitleHome" >
          <div class="col-12 col-lg-10 col-xl-8">
            <!-- Header -->
            <div class="header mt-md-5" style="margin-left:10px !important;margin-bottom:0px !important;">
              <div class="header-body">
                <div class="row align-items-center">
                  <div class="col">
                    <!-- Pretitle -->
                    <h6 class="header-pretitle">
                      {{userData.companyName}}
                    </h6>
                    <!-- Title -->
                    <h1 class="" style="font-size:35px; font-weight:700;">
                      ¡Bienvenido, {{userData.stName}}!
                    </h1>
                  </div>
                </div> <!-- / .row -->
              </div>
            </div>              
          </div>
        </div>

        
        <div class="row "  style="padding: 0 24px; margin-bottom:20px;">
          <div class="contHomeMenuData">
            <div>
              <div class="cardItemData" style="background: linear-gradient(-225deg, #625EB1 0%, #1D8FE1 48%,  #22E1Fa 100%);">
                <div class="cardItemDataItems">
                  <div style="display:flex;flex-direction:column;height:fit-content;width:fit-content;">
                    <span style="font-size:17px;font-weight:600;">
                      Ingresos totales
                    </span>
                    <span style="font-size:20px;font-weight:600;">
                      {{priceConverter(earnings)}}
                    </span>
                    
                  </div>
                  <img src="@/assets/profit.png" alt="">
                </div>
                
              </div>
            </div>

            <router-link style="cursor:pointer;" to="/orders">
              <div class="cardItemData" style="background: linear-gradient(-20deg, #fc6076 0%, #ff9a44 100%);">
                <div class="cardItemDataItems">
                  <div style="display:flex;flex-direction:column;height:fit-content;">
                    <span style="font-size:17px;font-weight:600;">
                      Pedidos
                    </span>
                    <span style="font-size:20px;font-weight:600;">
                      {{salesLength}}
                    </span>
                    
                  </div>
                  <img src="@/assets/give-money.png" alt="">
                </div>
              </div>
            </router-link>
            
            <router-link style="cursor:pointer;" to="/allclients">
              <div class="cardItemData" style="background: linear-gradient(45deg, #874da2 0%, #c43a30 100%);">
                <div class="cardItemDataItems">
                  <div style="display:flex;flex-direction:column;height:fit-content;">
                    <span style="font-size:17px;font-weight:600;">
                      Clientes
                    </span>
                    <span style="font-size:20px;font-weight:600;">
                      {{clientsLength}}
                    </span>
                    
                  </div>
                  <img src="@/assets/star.png" alt="">
                </div>
              </div>
            </router-link>

            <router-link style="cursor:pointer;" to="/orders/pending">
              <div class="cardItemData" style="background: linear-gradient(135deg, rgb(255, 168, 168) 10%, rgb(252, 255, 0) 100%);">
                <div class="cardItemDataItems">
                  <div style="display:flex;flex-direction:column;height:fit-content;">
                    <span style="font-size:17px;font-weight:600;">
                      Pedidos pendientes
                    </span>
                    <span style="font-size:20px;font-weight:600;">
                      {{ ventas.length }}
                    </span>
                    
                  </div>
                  <img src="@/assets/payment.png" alt="">
                </div>
              </div>
            </router-link>

            
          </div>
        </div>

        <div class="row "  style="padding: 0 24px; margin-bottom:20px;">
          <div class="contHomeMenu">
            <router-link :to="'/orders/pending'">
              <div class="cardItem">
                <img src="@/assets/file.png" alt="">
                <span>
                  Últimos pedidos
                </span>
              </div>
            </router-link>

            <router-link :to="'/products/create'">
              <div class="cardItem">
                <img src="@/assets/add-to-basket.png" alt="">
                <span>
                  Añadir producto
                </span>
              </div>
            </router-link>
            
            <router-link :to="'/products'">
              <div class="cardItem">
                <img src="@/assets/product.png" alt="">
                <span>
                  Buscar producto
                </span>
              </div>
            </router-link>
            
            <router-link :to="'/orders'">
              <div class="cardItem">
                <img src="@/assets/order-tracking.png" alt="">
                <span>
                  Buscar pedido
                </span>
              </div>
            </router-link>
            
          </div>
        </div>

        <div class="row gridFix"  style="padding: 0 20px;">
          <div class="col-12 " >

            <div class="header" style="margin:0px !important;">
              <div class="header-body" style="margin:0px !important; padding-bottom: 0!important">
                  <div class="row align-items-center">
                    <div class="col">
                        <!-- Title -->
                        <h1 class="header-titlee">
                          Últimos pedidos 
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
                          <th>Cliente</th>
                          <th>Pedido</th>
                          <th>Total</th>
                          <th>Estado</th>
                          <th></th>
                      </tr>
                  </thead>
                  <tbody class="fs-base" v-if="ventas.length >= 1">
                      <tr v-for="item in ventas">
                          <td>
                              <a>{{item.client.name}}</a>
                          </td>
                          <td>
                              <a>#{{item.orderNumber.toString().padStart(6,'000000')}}</a>
                          </td>
                          <td>
                              {{priceConverter(item.total)}}
                          </td>

                          <td>
                              <a>{{item.statusStr}}</a>
                          </td>

                          <td>
                              <router-link :to="{name: 'order',params: {id: item._id}}" class="btn-sm btn" style="background: linear-gradient(-50.6deg, rgb(179 134 149) -18.3%, rgb(67, 54, 74) 16.4%, rgb(47, 48, 67) 68.2%, rgb(69 41 90) 99.1%); color :#f9f9f9 ;">
                                  <span style="    color:#f9f9f9 !important;" >Editar</span>
                              </router-link>
                            
                          </td>
                      </tr>
                      
                  </tbody>
                  <tbody v-if="ventas.length == 0">
                      <tr>
                          <td colspan="4">
                              <div class="row justify-content-center">
                                      <div class="my-5 col-12 col-md-6 col-xl-6">
                                          
                                          <div class="text-center">
                                              <!-- Preheading -->
                                              <h6 class="mb-4 text-uppercase text-muted">
                                                  404 error
                                              </h6>

                                              <!-- Heading -->
                                              <h1 class="mb-3 display-4">
                                                  No se encontraron registros
                                              </h1>

                                              <!-- Subheading -->
                                              <p class="mb-4 text-muted">
                                                  No hay datos que mostrar!
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

<style>
.gridFix{
  padding-left: 130px !important;
}
.header-titlee{
  margin-left:0;
}
@media (max-width: 1500px){
  .gridFix{
    padding-left: 60px !important;
  }
}

@media (max-width: 1100px){
  .gridFix{
    padding-left: 20px !important;
  }
}
@media (max-width: 700px){
  .gridFix{
    padding-left: 0px !important;
  }
  .header-titlee{
    margin-left: 14px;
  }
}
</style>

<script>

import Sidebar from '@/components/Sidebar.vue'
import axios from 'axios';
import store from '@/store/index';
import currencyFormatter from 'currency-formatter'
import Loading from 'vue-loading-overlay';
import 'vue-loading-overlay/dist/vue-loading.css';

export default {
  name: 'Home',
  components: {
    Sidebar,
    Loading
  },
  data(){
        return {
            ventas: [],
            userData: {},
            userId: '',
            isLoading: true,
            clientsLength: '',
            salesLength: '',
            allSales: [],
            earnings: 0
        }
    },
    beforeMount(){
        this.getOrders()
        this.getClientsLength()
        this.getSalesLength()
        this.userId = localStorage.getItem('user_id')
        this.getUser()
    },
    methods: {
        priceConverter(price){
            return currencyFormatter.format(price, { code: 'ARS' });
        },
        getUser(){
            const token = localStorage.getItem('token')
            axios.get(this.$url+'/employees/'+this.userId,{
                headers:{
                        'Content-Type': 'application/json',
                        'Authorization': `Bearer ${token}`
                }
            }).then((result)=>{
                this.userData = result.data;
                this.userData.stName = this.userData.userName.split(' ')[0]
                this.isLoading = false
            });
        },
        getOrders(){
            axios.get(this.$url+'/orders/get/pending',{
                headers:{
                    'Content-Type': 'application/json',
                    'Authorization': `Bearer ${this.$token}`
                }
            }).then((result)=>{
                this.ventas = result.data.sort((a,b)=>{
                    if(a.createdAt < b.createdAt){
                        return -1
                    }
                })
            });
            
        },
        getClientsLength(){
            axios.get(this.$url+'/homedata/getclientslength',{
                headers:{
                    'Content-Type': 'application/json',
                    'Authorization': `Bearer ${this.$token}`
                }
            }).then((result)=>{
              this.clientsLength = result.data
            });
        },
        getSalesLength(){
            axios.get(this.$url+'/homedata/getsaleslength',{
                headers:{
                    'Content-Type': 'application/json',
                    'Authorization': `Bearer ${this.$token}`
                }
            }).then((result)=>{
                this.allSales = result.data
                this.salesLength = result.data.length
                this.getTotalEarnings()
            });
        },
        getTotalEarnings(){
            let totalEarnings = 0
            let data = this.allSales.map((sale) => {
                const saleData = Object.entries(sale)
                const saleTotal = saleData[1]
                const total = saleTotal[1]
                totalEarnings = totalEarnings + total                  
            })
            this.earnings = totalEarnings
        },

    },
    mounted(){
        window.scrollTo(0, 0)
    },
}
</script>
