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
                                        Precios en tienda
                                        </h1>
                                    </div>

                                </div> <!-- / .row -->
                                
                            </div>
                        </div>

                        <div>
                            <div class="row">
                                <div class="col-12 col-md-6">
                                    <div class="form-group">
                                        <label class="form-label">
                                            Cotización del dólar
                                        </label>

                                        <input type="number" class="form-control" placeholder="Ingresa un precio" v-model="USDData.value" >
                                    </div>
                                </div>

                                <div class="col-12 col-md-6">
                                    <div class="form-group">
                                        <label class="form-label">
                                            Mostrar precio
                                        </label>

                                        <select name="" class="form-select" v-model="USDData.enabled" >
                                            <option value=true >Original del producto</option>
                                            <option value=false >Sujeto al dólar</option>
                                        </select>
                                    </div>
                                </div>

                                <div class="col-12 col-md-6">
                                    <div class="form-group" style="display:flex;align-items:center; height:fit-content">
                                        <img src="@/assets/clock-rotate-left-solid.svg" style="width:11px;">
                                        <router-link to="/usdlogs" class="" style="cursor:pointer; color:black; font-size:15px; margin-left:5px;">
                                            Historial de cotizaciones del dólar
                                        </router-link>
                                    </div>
                                </div>
                            

                            </div> 
                                                
                            <button class="btn" style="background: linear-gradient(-50.6deg, rgb(179 134 149) -18.3%, rgb(67, 54, 74) 16.4%, rgb(47, 48, 67) 68.2%, rgb(69 41 90) 99.1%); color :#f9f9f9 ;"  v-on:click="validate()" >
                                Guardar cambios
                            </button>
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

export default {
    name: "USDSettings",
    components: {
        Sidebar,
        Loading
    },
    data(){
        return {
            USDData: {},
        }
    },
    beforeMount(){
        this.getUSDSettings()
    },
    mounted(){
        window.scrollTo(0, 0)
    },
    methods: {
        validate(){
            if(this.USDData.value == ''){
                return this.$notify({
                    group: 'foo',
                    title: 'Error',
                    text: 'Completar todos los campos',
                    type: 'error',
                    position: 'top center',
                    duration: 100000,
                    speed: 1000
                });
            } 
            if(this.USDData.enabled === "true"){
                this.USDData.enabled = true
            }
            if(this.USDData.enabled === "false"){
                this.USDData.enabled = false
            }
            this.saveUSDSetting()            
        },
        getUSDSettings(){
            const user = localStorage.getItem('user_id')
            axios.get(`${this.$url}/config/getusdsettings`, {
                headers: {
                    "Content-Type": 'application/json',
                    "Authorization" : `Bearer ${this.$token}`
            }
            }).then((response) => {
                this.USDData = response.data[0]
            }).catch( error => {
                console.log(error)
            })
        },
        saveUSDSetting(){
            axios.put(`${this.$url}/config/usdsettings`, this.USDData, {
                headers: {
                    "Content-Type": 'application/json',
                    "Authorization" : `Bearer ${this.$token}`
                }
            }).then((response) => {
                const {data} = response
                this.getUSDSettings()
                this.$notify({
                    group: 'foo',
                    title: '',
                    text: 'Configuraciones actualizadas correctamente!',
                    type: 'success'
                });
                this.getUSDSettings()
            }).catch( error => {
                console.log(error)
                this.msm_error = error.response.data.msg
                this.$notify({
                    group: 'foo',
                    title: 'Error',
                    text: error.response.data.msg,
                    type: 'error'
                });
            })
        },
    }
}
</script>