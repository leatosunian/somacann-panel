<template>
  <div>
    <template>
      <div class="vld-parent" style="z-index: 999999 !important">
        <loading
          :active.sync="isLoading"
          :can-cancel="false"
          :is-full-page="true"
          :opacity="0.61"
        />
      </div>
    </template>
    <Sidebar />
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
                    <h6 class="header-pretitle">Mis Pedidos</h6>

                    <!-- Title -->
                    <h1 class="header-title">
                      <b>Pedidos pendientes</b>
                    </h1>
                  </div>
                </div>
                <!-- / .row -->
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
                        <a>{{ item.client.name }}</a>
                      </td>
                      <td>
                        <a
                          >#{{
                            item.orderNumber.toString().padStart(6, "000000")
                          }}</a
                        >
                      </td>
                      <td>
                        {{ priceConverter(item.total) }}
                      </td>

                      <td>
                        <a>{{ item.statusStr }}</a>
                      </td>

                      <td>
                        <router-link
                          :to="{ name: 'order', params: { id: item._id } }"
                          class="btn-sm btn"
                          style="background: rgb(67, 54, 74); color: #f9f9f9"
                        >
                          <span style="color: #f9f9f9 !important">Editar</span>
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
        <!-- / .row -->
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Sidebar from "@/components/Sidebar.vue";
import currencyFormatter from "currency-formatter";
import Loading from "vue-loading-overlay";
import "vue-loading-overlay/dist/vue-loading.css";

export default {
  name: "PendingOrders",
  components: {
    Sidebar,
    Loading,
  },
  data() {
    return {
      ventas: [],
      isLoading: true,
    };
  },
  beforeMount() {
    this.getOrders();
  },
  methods: {
    priceConverter(price) {
      return currencyFormatter.format(price, { code: "ARS" });
    },
    getOrders() {
      axios
        .get(this.$url + "/orders/get/pending", {
          headers: {
            "Content-Type": "application/json",
            Authorization: `Bearer ${this.$token}`,
          },
        })
        .then((result) => {
          this.ventas = result.data.sort((a, b) => {
            if (a.createdAt < b.createdAt) {
              return -1;
            }
          });
          this.isLoading = false;
        });
    },
  },
};
</script>
