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
                    <h6 class="header-pretitle">Mis Productos</h6>

                    <!-- Title -->
                    <h1 class="header-title">Editar Producto</h1>
                  </div>
                </div>
                <!-- / .row -->
                <div class="row align-items-center">
                  <div class="col">
                    <!-- Nav -->
                    <ul class="nav nav-tabs nav-overflow header-tabs">
                      <li class="nav-item">
                        <router-link to="/products" class="nav-link"
                          >Todos los productos</router-link
                        >
                      </li>

                      <li class="nav-item">
                        <router-link class="nav-link active" to=""
                          >Nuevo Producto</router-link
                        >
                      </li>
                    </ul>
                  </div>
                </div>
              </div>
            </div>

            <div>
              <div class="row">
                <div class="col-12 col-md-6">
                  <!-- Email address -->
                  <div class="form-group">
                    <!-- Label -->
                    <label
                      class="mb-1"
                      style="font-size: 17px; font-weight: 500"
                    >
                      Nombre del producto
                    </label>

                    <!-- Input -->
                    <input
                      type="text"
                      class="form-control"
                      placeholder="Nombre del producto"
                      v-model="product.name"
                    />
                  </div>
                </div>

                <div class="col-12 col-md-6">
                  <!-- Last name -->
                  <div class="form-group">
                    <!-- Label -->
                    <label class="form-label"> Precio </label>

                    <!-- Input -->
                    <input
                      type="number"
                      class="form-control"
                      placeholder="Ingresa el precio"
                      v-model="product.price"
                    />
                  </div>
                </div>

                <div class="col-12 col-md-6">
                  <!-- First name -->
                  <div class="form-group">
                    <!-- Label -->
                    <label class="form-label"> Categoria </label>

                    <!-- Input -->
                    <select
                      name=""
                      class="form-select"
                      v-model="product.category"
                      v-on:change="getSubcategories($event)"
                    >
                      <option value="" disabled selected>Seleccionar</option>
                      <option
                        :value="item.category.name"
                        v-for="item in categories"
                      >
                        {{ item.category.name }}
                      </option>
                    </select>
                  </div>
                </div>

                <div class="col-12 col-md-6">
                  <!-- First name -->
                  <div class="form-group">
                    <!-- Label -->
                    <label class="form-label"> Subcategoria </label>

                    <!-- Input -->
                    <select
                      name=""
                      class="form-select"
                      v-model="product.subcategory"
                    >
                      <option value="" disabled selected>Seleccionar</option>
                      <option :value="item.name" v-for="item in subcategories">
                        {{ item.name }}
                      </option>
                    </select>
                  </div>
                </div>

                <div class="col-12 col-md-6">
                  <!-- Last name -->
                  <div class="form-group">
                    <!-- Label -->
                    <label class="mb-1 form-label">Variedad</label
                    ><span
                      style="margin-left: 5px; font-size: 12px; color: #95aac9"
                      >(ej. Color)</span
                    >
                    <!-- Input -->
                    <input
                      type="text"
                      class="form-control"
                      placeholder="Ingresa las variantes (ej. color)"
                      v-model="product.str_variant"
                    />
                  </div>
                </div>

                <div class="col-12 col-md-6" v-if="variants.length == 0">
                  <!-- Last name -->
                  <div class="form-group">
                    <!-- Label -->
                    <label class="mb-1 form-label">Stock</label
                    ><span
                      style="margin-left: 5px; font-size: 12px; color: #95aac9"
                      >(sin variantes)</span
                    >
                    <!-- Input -->
                    <input
                      type="text"
                      class="form-control"
                      placeholder="Ingresa las variantes (ej. color)"
                      v-model="product.stock"
                    />
                  </div>
                </div>

                <div class="col-12 col-md-6">
                  <!-- Phone -->
                  <div class="form-group">
                    <!-- Label -->
                    <label class="form-label"> Descripción </label>

                    <!-- Input -->
                    <textarea
                      class="form-control"
                      id=""
                      rows="3"
                      placeholder="Ingresa una descripción"
                      v-model="product.description"
                    ></textarea>
                  </div>
                </div>

                <div class="row justify-content-between align-items-center">
                  <div class="col">
                    <div class="row align-items-center">
                      <div class="col-auto">
                        <!-- Avatar -->
                        <div class="avatar">
                          <img
                            class="avatar-img rounded-3"
                            :src="str_image"
                            alt=""
                          />
                        </div>
                      </div>
                      <div class="col ms-n2">
                        <!-- Heading -->
                        <h4 class="mb-1">
                          <b>Imagen</b>
                        </h4>

                        <!-- Text -->
                        <small class="text-muted">
                          En formato PNG, JPG, JPEG, webp y menor a 2MB.
                        </small>
                      </div>
                    </div>
                    <!-- / .row -->
                  </div>
                  <div class="col-4">
                    <!-- Button -->

                    <label
                      for="file-upload"
                      class="btn btn-sm"
                      style="background: rgb(47, 48, 67) 68.2%; color: white"
                    >
                      Cambiar imagen
                    </label>
                    <input
                      style="display: none"
                      id="file-upload"
                      type="file"
                      v-on:change="uploadImg($event)"
                    />
                  </div>
                </div>
                <!-- / .row -->
              </div>
              <!-- / .row -->

              <div class="row" style="margin-top: 50px">
                <div class="col-sm-12 col-md-6">
                  <!-- Public profile -->
                  <div class="form-group">
                    <!-- Label -->
                    <label class="mb-1"> Producto publicado </label>

                    <!-- Form text -->
                    <small class="form-text text-muted">
                      Agrega o quita el producto de la tienda
                    </small>

                    <div class="row">
                      <div class="col-auto">
                        <!-- Switch -->
                        <div class="form-check form-switch">
                          <input
                            class="form-check-input"
                            type="checkbox"
                            id="switchOne"
                            style="
                              -webkit-tap-highlight-color: transparent;
                              user-select: none;
                              -webkit-touch-callout: none;
                            "
                            v-model="product.forSale"
                          />
                          <label class="form-check-label" for="switchOne">
                          </label>
                        </div>
                      </div>
                      <div class="col ms-n2">
                        <!-- Help text -->
                        <small class="text-muted" v-if="product.forSale">
                          Producto activado
                        </small>

                        <small class="text-muted" v-if="!product.forSale">
                          Producto desactivado
                        </small>
                      </div>
                    </div>
                    <!-- / .row -->
                  </div>
                </div>
                <div class="col-12 col-md-6"></div>
              </div>
              <!-- / .row -->

              <!-- Divider || VARIANTS SECTION -->
              <hr class="mt-4 mb-5" />

              <div class="mb-5 row justify-content-between align-items-center">
                <div class="col-12">
                  <h2 class="mb-2">Variedades del producto</h2>

                  <p class="text-muted mb-xl-0">
                    Agrega un color, un talle o las variantes de tu producto
                  </p>
                </div>
              </div>

              <div class="mb-5 row">
                <div class="col-lg-4">
                  <small class="text-muted"> Variedad </small>
                  <input
                    type="text"
                    class="form-control"
                    placeholder="ej. Rojo"
                    v-model="variant.variant"
                  />
                </div>
                <div class="col-lg-2">
                  <small class="text-muted"> Stock </small>
                  <input
                    type="number"
                    class="form-control"
                    placeholder="Cantidad"
                    v-model="variant.stock"
                  />
                </div>
                <div class="col-sm-4 col-md-3 col-lg-2 col-4">
                  <br />
                  <button
                    class="btn btn-block"
                    style="
                      background: rgb(47, 48, 67) 68.2%;
                      color: white;
                      width: 100% !important;
                    "
                    v-on:click="validateVariant()"
                  >
                    Agregar
                  </button>
                </div>
              </div>

              <div class="card">
                <div class="card-body" style="padding: 0 10px !important">
                  <div
                    class="list-group list-group-flush"
                    v-for="item in variants"
                  >
                    <div class="list-group-item">
                      <div
                        class="row align-items-center"
                        style="margin: auto 0 !important"
                      >
                        <div class="col">
                          <h4 class="mb-1" style="font-weight: 600 !important">
                            {{ item.variant }}
                          </h4>

                          <small class="text-muted">
                            Codigo del producto: {{ item.skuCode }}
                          </small>
                        </div>

                        <div class="col">
                          <h4 class="mb-1"><b>Stock:</b> {{ item.stock }}</h4>
                        </div>

                        <div class="col-auto" style="padding: 0">
                          <button
                            class="btn btn-sm"
                            style="
                              background: rgb(47, 48, 67) 68.2%;
                              color: white;
                            "
                            type="button"
                            @click="selectEditVariant(item._id)"
                            v-b-modal="'edit-' + item._id"
                          >
                            Editar
                          </button>

                          <b-modal
                            centered
                            :id="'edit-' + item._id"
                            title="BootstrapVue"
                            title-html="<h4 class='card-header-title'><b>Editar variedad</b></h4>"
                            @ok="editVariant(item)"
                          >
                            <p
                              class=""
                              style="font-weight: 600; font-size: 20px"
                            >
                              {{ item.variant }}
                            </p>

                            <div class="">
                              <small class="text-muted"> Variedad </small>
                              <input
                                type="text"
                                class="form-control"
                                style="width: 100%"
                                placeholder="ej. Rojo"
                                v-model="editedVariant.variant"
                              />
                            </div>
                            <div class="">
                              <small class="text-muted"> Stock </small>
                              <input
                                type="number"
                                class="form-control"
                                style="width: 100%"
                                placeholder="Cantidad"
                                v-model="editedVariant.stock"
                              />
                            </div>
                          </b-modal>
                        </div>

                        <div class="col-auto">
                          <button
                            class="btn btn-sm btn-danger"
                            type="button"
                            v-b-modal="'delete-' + item._id"
                          >
                            Eliminar
                          </button>

                          <b-modal
                            centered
                            :id="'delete-' + item._id"
                            title="BootstrapVue"
                            title-html="<h4 class='card-header-title'><b>Eliminar variedad</b></h4>"
                            @ok="deleteVariant(item._id)"
                          >
                            <p class="my-4">
                              ¿Querés eliminar la variedad {{ item.variant }}?
                            </p>
                          </b-modal>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
                <!-- Divider -->
                <hr class="mt-5 mb-4" />

                <!-- Button -->
                <button
                  class="btn col-sm-5 col-md-5 col-lg-3"
                  style="background: rgb(47, 48, 67) 68.2%; color: white"
                  v-on:click="validate()"
                >
                  Guardar cambios
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <hr class="mt-5 mb-6" />
      <!-- / .row -->
    </div>
  </div>
</template>
<style>
.nav-tabs .nav-item.show .nav-link,
.nav-tabs .nav-link.active {
  background-color: transparent;
  border-color: transparent transparent rgb(87 36 114);
  color: #12263f;
}

.form-check-input:checked {
  background-color: rgb(81 50 98);
  border-color: #cfcfcf;
  -webkit-tap-highlight-color: transparent;
}

.form-check-input {
  background-color: rgba(81, 50, 98, 0.37);
  -webkit-tap-highlight-color: transparent;
}
</style>
<script>
import Sidebar from "@/components/Sidebar.vue";
import axios from "axios";
import Loading from "vue-loading-overlay";
import "vue-loading-overlay/dist/vue-loading.css";
import store from "@/store/index";

// @ is an alias to /src
export default {
  name: "EditProduct",
  components: {
    Sidebar,
    Loading,
  },

  data() {
    return {
      str_image: "",
      product: {
        image: undefined,
      },
      uploadedImg: undefined,
      variant: {},
      editedVariant: {
        variant: "",
        stock: 0,
        _id: "",
      },
      variants: [],
      categories: [],
      subcategories: [],
      isLoading: true,
    };
  },

  methods: {
    selectEditVariant(id) {
      const variantToEdit = this.variants.find((variant) => variant._id == id);
      this.editedVariant = variantToEdit;
    },
    getProducts() {
      const token = localStorage.getItem("token");
      axios
        .get(this.$url + "/products/get/" + this.$route.params.id, {
          headers: {
            "Content-Type": "application/json",
            Authorization: `Bearer ${token}`,
          },
        })
        .then((response) => {
          const { data } = response;
          this.product = data;
          this.str_image = this.product.image;
        })
        .catch((error) => {
          console.log(error);
          this.msm_error = error.response.data.msg;
        });
    },
    uploadImg($event) {
      var image;

      if ($event.target.files.length >= 1) {
        image = $event.target.files[0];
      }

      if (image.size <= 2480000) {
        if (
          image.type == "image/jpeg" ||
          image.type == "image/png" ||
          image.type == "image/webp" ||
          image.type == "image/jpg"
        ) {
          this.str_image = URL.createObjectURL(image);
          this.uploadedImg = image;
          this.product.image = this.uploadedImg;
        } else {
          this.$notify({
            group: "foo",
            title: "Error",
            text: "El archivo debe ser una imagen",
            type: "error",
          });
          this.image = undefined;
        }
      } else {
        this.$notify({
          group: "foo",
          title: "Error",
          text: "La imagen es muy pesada, máximo 1 megabyte",
          type: "error",
        });
        this.image = undefined;
      }
    },
    validate() {
      if (
        !this.product.name ||
        !this.product.price ||
        !this.product.image ||
        !this.product.category ||
        !this.product.subcategory
      ) {
        return this.$notify({
          group: "foo",
          title: "Error",
          text: "Completar todos los campos",
          type: "error",
        });
      }
      this.editProduct();
    },
    editProduct() {
      let data;
      let content = "";
      if (this.image != undefined) {
        content = "multipart/form-data";
        data = new FormData();
        formData.append("name", this.product.name);
        formData.append("description", this.product.description);
        formData.append("stock", this.product.stock);
        formData.append("price", this.product.price);
        formData.append("category", this.product.category);
        formData.append("subcategory", this.product.subcategory);
        formData.append("image", this.product.image);
        formData.append("forSale", this.product.forSale);
        formData.append("stock", this.product.stock);
      } else {
        content = "application/json";
        data = this.product;
      }

      const token = localStorage.getItem("token");

      axios
        .put(this.$url + "/products/edit/" + this.$route.params.id, data, {
          headers: {
            "Content-Type": "multipart/form-data",
            Authorization: `Bearer ${token}`,
          },
        })
        .then((response) => {
          const { data } = response;
          this.$notify({
            group: "foo",
            title: "",
            text: "Producto editado correctamente!",
            type: "success",
          });
        })
        .catch((error) => {
          console.log(error);
          this.msm_error = error.response.data.msg;
          return this.$notify({
            group: "foo",
            title: "Error",
            text: error.response.data.msg,
            type: "error",
          });
        });

      setTimeout(() => {
        this.$router.push({ name: "products" });
      }, 3000);
    },

    SKUGen() {
      const SKU =
        this.product.name.substr(0, 3) +
        "" +
        this.product.str_variant.substr(0, 3) +
        "" +
        this.variant.variant.substr(0, 3);
      return SKU.toUpperCase();
    },

    validateVariant() {
      if (!this.variant.variant) {
        this.$notify({
          group: "foo",
          title: "Error",
          text: "Completar todos los campos",
          type: "error",
        });
      }

      if (this.product.str_variant == undefined) {
        this.$notify({
          group: "foo",
          title: "Error",
          text: "Ingresa el tipo de variedades del producto",
          type: "error",
        });
      }
      this.variant.provider = "none";
      this.variant.product = this.$route.params.id;
      this.variant.skuCode = this.SKUGen();
      this.saveVariant();
    },

    saveVariant() {
      const token = localStorage.getItem("token");
      axios
        .post(this.$url + "/variants/save", this.variant, {
          headers: {
            "Content-Type": "application/json",
            Authorization: `Bearer ${token}`,
          },
        })
        .then((response) => {
          const { data } = response;

          this.variant = {};
          this.getVariants();
          this.$notify({
            group: "foo",
            title: "",
            text: "Variedad añadida correctamente!",
            type: "success",
          });
        })
        .catch((error) => {
          console.log(error);
          this.msm_error = error.response.data.msg;
          return this.$notify({
            group: "foo",
            title: "Error",
            text: error.response.data.msg,
            type: "error",
          });
        });
    },

    editVariant(id) {
      const token = localStorage.getItem("token");

      const body = {
        _id: id,
        variant: this.editedVariant.variant,
        stock: Number(this.editedVariant.stock),
      };

      axios
        .put(this.$url + "/variants/edit", body, {
          headers: {
            "Content-Type": "application/json",
            Authorization: `Bearer ${token}`,
          },
        })
        .then((response) => {
          this.$notify({
            group: "foo",
            title: "",
            text: "Variedad editada correctamente!",
            type: "success",
          });
          this.getVariants();
        })
        .catch((error) => {
          console.log(error);
          this.msm_error = error.response.data.msg;
          return this.$notify({
            group: "foo",
            title: "Error",
            text: "No se pudo editar la variedad",
            type: "error",
          });
        });
    },

    getVariants() {
      const token = localStorage.getItem("token");
      axios
        .get(this.$url + "/variants/" + this.$route.params.id, {
          headers: {
            "Content-Type": "application/json",
            Authorization: `Bearer ${token}`,
          },
        })
        .then((response) => {
          const { data } = response;
          this.variants = data;
        })
        .catch((error) => {
          console.log(error.response.data.msg);
          this.msm_error = error.response.data.msg;
          return this.$notify({
            group: "foo",
            title: "Error",
            text: error.response.data.msg,
            type: "error",
          });
        });
    },

    deleteVariant(id) {
      const token = localStorage.getItem("token");
      axios
        .delete(this.$url + "/variants/variant/" + id, {
          headers: {
            "Content-Type": "application/json",
            Authorization: `Bearer ${token}`,
          },
        })
        .then((response) => {
          this.getVariants();
          this.getProducts();
          this.$notify({
            group: "foo",
            title: "",
            text: "Variedad eliminada!",
            type: "success",
          });
        })
        .catch((error) => {
          console.log(error.response.data.msg);
          this.msm_error = error.response.data.msg;
          return this.$notify({
            group: "foo",
            title: "Error",
            text: error.response.data.msg,
            type: "error",
          });
        });
    },
    getCategories() {
      const token = localStorage.getItem("token");
      axios
        .get(this.$url + "/getAllCategories", {
          headers: {
            "Content-Type": "application/json",
            Authorization: `Bearer ${token}`,
          },
        })
        .then((response) => {
          const { data } = response;
          this.categories = data;
          this.isLoading = false;
          this.subcategories = this.categories.filter(
            (cat) => cat.category.name == this.product.category
          )[0].subcategories;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getSubcategories(event) {
      this.subcategories = this.categories.filter(
        (cat) => cat.category.name == event.target.value
      )[0].subcategories;
    },
  },
  beforeMount() {
    this.getCategories();
    this.getProducts();
    this.getVariants();
    this.getSubcategories();
  },
  mounted() {
    window.scrollTo(0, 0);
  },
};
</script>
