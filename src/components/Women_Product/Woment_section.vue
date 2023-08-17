<!-- eslint-disable vue/no-unused-components -->
<script>
import Button from "@/components/Button/Button.vue";
import { RouterLink } from "vue-router";

export default {
  name: "WomenElips",
  components: {
    Button,
    RouterLink,
  },
  props: {},
  data() {
    return {
      listData: [],
      listdataY: [],
      categoryActive: "men's clothing",
      categories: ["men's clothing", "women's clothing", "jewelery"],
      search: "",
      filterProduct: [],
      newProduct: {
        title: "",
        category: "",
        rating: "",
        description: "",
        price: 0,
        image: "",
      },
      // set index untuk custom data from Api
      currentIndexProduct: 0,
      ElipsData: [
        {
          svg:
            '<svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 18 18" fill="none">' +
            '<circle cx="9" cy="9" r="9" fill="" />' +
            "</svg>",
        },
      ],
    };
  },
  computed: {
    currentProduct() {
      if (this.filterProduct.length > 0) {
        return this.filterProduct[this.currentIndexProduct];
      } else {
        return null;
      }
    },
  },
  methods: {
    ShowIncrementData() {
      this.currentIndexProduct++;
      if (this.currentIndexProduct >= this.listData.length) {
        // set ke index awal
        this.currentIndexProduct = 0;
      }
    },
    GetAllwithSearch() {
      const API = "https://fakestoreapi.com/products?sort=title";
      fetch(API)
        .then((response) => response.json())
        .then((res) => {
          this.listData = res;
        });
    },
    SearchApiQuery() {
      this.filterProduct = this.listData.filter((s) =>
        s.title.toLowerCase().includes(this.search.toLowerCase())
      );
      this.search = "";
    },
    FilterApidata() {
      const FilterDataWithAPi = this.categories;
      this.listData = this.listData.filter((products) =>
        FilterDataWithAPi.includes(products.category)
      );
    },
    ClassBinding(category) {
      if (category === "men's clothing") {
        return "blue";
      } else if (category === "women's clothing") {
        return "purple";
      }
    },
    async addNewProduct(category) {
      this.newProduct.category = category;
      try {
        const API_URL_ADD = await fetch("https://fakestoreapi.com/products", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(this.newProduct),
        });

        const newProductData = await API_URL_ADD.json();
        this.listData.push(newProductData);
        console.log("data di tambahkan", newProductData);

        await this.refreshProductList();
      } catch (error) {
        console.log("error", error);
      }
    },
    async refreshProductList() {
      try {
        const response = await fetch("https://fakestoreapi.com/products");
        const products = await response.json();
        this.listData = products; // Update your products data
      } catch (error) {
        console.error("Error refreshing product list:", error);
      }
    },
    // create class binding untuk background kategori yang aktif
    ClassBindingStyle(category) {
      if (category === "men's clothing") {
        return "mensClothing_blue";
      } else if (category === "women's clothing") {
        return "womens-Clothing_purple";
      }
    },
    ButtonBinding(category) {
      if (category === "men's clothing") {
        return "button_blue";
      } else if (category === "women's clothing") {
        return "button_purple";
      }
    },
  },
  created() {
    this.GetAllwithSearch();
  },
  mounted() {
    const API_URL = "https://fakestoreapi.com/products";
    fetch(API_URL)
      .then((response) => {
        if (!response.ok) {
          throw new Error("API tidak terhubung");
        }
        return response.json();
      })
      .then((data) => {
        console.log(data);
        this.listData = data;
        this.FilterApidata();
        this.refreshProductList();
        this.SearchApiQuery();
      })
      .catch((error) => {
        console.log(error);
      });
  },
};
</script>

<template>
  <section class="Container_categori_section" :class="ClassBindingStyle">
    <div>
      <div class="Container_layout">
        <div>
          <!-- start -->
          <div class="Shadow-box">
            <div class="InputSearch">
              <input
                type="text"
                v-model="search"
                alt=""
                placeholder="Input search from product with api"
              />
              <div class="button_SeacrhQuery">
                <button @click="SearchApiQuery">Search Product</button>
              </div>
            </div>
            <div class="AddProductForm">
              <h2>Add New Product</h2>
              <form @submit.prevent="addNewProduct('jewelery')">
                <label for="title">Title:</label>
                <input
                  v-model="newProduct.title"
                  type="text"
                  id="title"
                  required
                />
                <label for="price">price</label>
                <input
                  v-model="newProduct.price"
                  type="text"
                  id="price"
                  required
                />
                <label for="price">rating</label>
                <input
                  v-model="newProduct.rating"
                  type="text"
                  id="rating"
                  required
                />
                <label for="description">description</label>
                <input
                  v-model="newProduct.description"
                  type="text"
                  id="description"
                  required
                />
                <label for="image">image</label>
                <input
                  v-model="newProduct.image"
                  type="url"
                  id="image"
                  required
                />
                <label for="category">category</label>
                <input
                  v-model="newProduct.category"
                  type="hidden"
                  id="category"
                  value="jewelery"
                  required
                />
                <!-- ... other input fields for price, description, image, and category ... -->
                <button type="submit">Add Product</button>
              </form>
            </div>
            <div class="Shadow_box2">
              <div class="Women">
                <!-- tampilkan product jika berada pada pencarian {v-if = "currentProduct"}-->
                <div class="Women_1" v-if="currentProduct">
                  <div class="Women_Card_Display_1">
                    <!-- image -->
                    <div class="image_product">
                      <div class="image_product2">
                        <!-- pastikan current memiliki data sebelum meng akses dari properti -->
                        <img
                          v-if="currentProduct.image"
                          :src="currentProduct.image"
                          alt=""
                          width="200px"
                        />
                      </div>
                    </div>
                  </div>
                  <!-- detail -->
                  <div class="Women_Card_Display_2">
                    <div class="Text_header">
                      <h4 :class="ClassBinding(currentProduct.category)">
                        {{ currentProduct.title }}
                      </h4>
                      <div class="Women_clothing">
                        <p :class="ClassBinding(currentProduct.category)">
                          {{ currentProduct.category }}
                        </p>
                        <li v-for="elips in ElipsData" :key="elips.Elips">
                          <h5 :class="ClassBinding(currentProduct.category)">
                            {{ currentProduct.rating.rate }}
                          </h5>
                          <div
                            class="svg-wrap"
                            :class="ClassBinding(currentProduct)"
                          >
                            <div class="svg-fill" v-html="elips.svg"></div>
                          </div>
                        </li>
                      </div>
                      <hr size="1" width="100%" align="left" color="black" />
                      <div class="Women_clothing2">
                        <p :class="ClassBinding(currentProduct.category)">
                          {{ currentProduct.description }}
                        </p>
                      </div>
                      <div class="Price_Product">
                        <br />
                        <hr
                          size="0.1"
                          width="100%"
                          align="left"
                          color="black"
                        />
                      </div>
                      <!-- button -->
                      <div class="Button_Products">
                        <h2 :class="ClassBinding(currentProduct.category)">
                          ${{ currentProduct.price }}
                        </h2>
                        <!-- use event Emit -->
                        <Button
                          @next-product="ShowIncrementData"
                          :class="ButtonBinding(currentProduct.category)"
                        />
                      </div>
                    </div>
                  </div>
                </div>
                <div v-else>product not found</div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <router-link to="/">Home</router-link> |
    </div>
  </section>
</template>

<style>
:root {
  /* color: #720060; */
  --purple: purple;
  --blue: #002772;
  --blueyoung: #d6e6ff;
  --white: #fff;
  --pink: #fde2ff;
}
.InputSearch {
  margin: 30px;
}
.button_SeacrhQuery button {
  width: 15%;
  padding: 2px;
  margin-top: 10px;
  background-color: #002772;
}
.Container_categori_section {
  width: 100%;
  height: 500px;
  flex-shrink: 0;
  background-color: var(--blueyoung);
}
.Shadow-box {
  padding: 5rem;
}
h1 {
  margin-top: 8rem;
}
.image_product {
  width: 12rem;
  margin: 0 auto;
  position: relative;
  text-align: center;
}
.image_product2 {
  width: 13.5rem;
  margin-top: 5rem;
  width: 100%;
}

.Container_layout {
  position: relative;
}
.Women {
  border: 1px solid black;
  width: 900px;
  padding: 10px;
  justify-content: center;
  align-items: center;
  display: flex;
  background-color: var(--white);
  /* agar shadow box berada di tengah */
  margin: 0 auto;
}
.Women_1 {
  display: flex;
  width: 100%;
}
.Women_Card_Display_1 {
  flex-basis: 35%;
}
.Women_Card_Display_2 {
  flex-basis: 75%;
  background-color: var(--white);
  height: 500px;
}
/* flex basis 75% */
.Women_clothing {
  width: 100%;
  flex-direction: row;
  display: flex;
  align-items: center;
  justify-content: end;
}
.Women_clothing2 {
  width: 100%;
  flex-direction: column;
}
.Women_clothing2 p {
  width: 100%;
  font-size: 15px;
  box-sizing: border-box;
}
.Text_header {
  padding: 1rem;
}
p {
  font-size: 10px;
  margin-right: 23rem;
}
li {
  list-style: none;
  display: flex;
  align-items: center;
  margin-right: 3px;
}
h5 {
  font-size: 9px;
}
.Price_Product {
  margin-top: 5rem;
}
/* classbinding */
.purple {
  color: var(--purple);
}

.blue {
  color: var(--blue);
}
/* ---------------- */
.mensClothing_blue {
  background: var(--blueyoung);
}
.womens-Clothing_purple {
  background: var(--pink);
}
/* blue */
.button_blue button:nth-child(1) {
  background-color: var(--blue);
  border: 3px solid var(--blue);
}
.button_blue button:nth-child(2) {
  color: var(--blue);
}
.button_blue button {
  border: 3px solid var(--blue);
}
/* purple */
.button_purple button:nth-child(1) {
  background-color: var(--purple);
  border: 3px solid var(--purple);
}
.button_purple button:nth-child(2) {
  color: var(--purple);
}
.button_purple button {
  border: 3px solid var(--purple);
}
/* SVG */
.mensClothing_blue .svg-fill {
  stroke-width: 1px;
  fill: #002772;
  stroke: #002772;
}
.blues {
  fill: blue;
}
</style>
