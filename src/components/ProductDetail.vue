<template>
  <div>
    <div class="main-container">
      <div class="detail-img">
        <img :src="productImage" />
      </div>
      <div class="detail-base">
        <div class="mb-2 class-color class-size">{{product.productBrand}}</div>
        <div>{{product.productName}}</div>
        <div class="mt-4 class-size">Rs. {{product.price}}</div>
        <div>Price inclusive of all taxes</div>
        <div class="mt-4">Size</div>
        <div class="mt-2">{{product.size}}</div>
        <div
          class="mt-4"
          v-if="this.$store.state.auth.role === 'customer' && this.$store.getters.isAuthenticated"
        >
          <v-btn elevation="2" large @click="addToCart(product)">Add to cart</v-btn>
        </div>
        <div style="margin-top:50px">
          <div class="mt-8 mb-4 class-color" style="font-weight:500; font-size:20px">
            <h4>Product Details</h4>
          </div>
          <ul>
            <li v-for="(productD,index) in product.productDetails" :key="index">{{productD}}</li>
          </ul>
        </div>
      </div>
    </div>
    <AppFooter />
  </div>
</template>

<script>
import { singleProduct } from "@/services/product";
import Vue from "vue";
import AppFooter from "@/components/AppFooter.vue";
import { showMe } from "@/services/user";
export default {
  name: "ProductDetail",
  components: {
    AppFooter
  },
  data() {
    return {
      product: {},
      singleItem: {
        productName: "",
        imageUrl: "",
        price: "",
        productId: ""
      },
      cart: {
        userId: "",
        cartIteams: []
      }
    };
  },
  methods: {
    addToCart(product) {
      this.singleItem.productName = product.productName;
      this.singleItem.imageUrl = product.imageUrl;
      this.singleItem.price = product.price;
      this.singleItem.productId = product._id;
      Vue.$toast.open({
        message: "Product added to cart",
        duration: 3000,
        type: "success"
      });
      showMe().then(data => {
        this.cart.userId = data.user.userId;

        this.$store.state.auth.cartItems.push(this.singleItem);
        this.$store.commit("setCartItem", this.$store.state.auth.cartItems);
      });
    }
  },
  computed: {
    productImage() {
      return this.$store.state.auth.productImage;
    }
  },
  created() {
    singleProduct(
      this.$store.state.auth.storeId,
      this.$store.state.auth.productId
    )
      .then(data => {
        console.log("storeid", this.$store.state.auth.storeId);
        console.log("productid", this.$store.state.auth.productId);

        this.product = data[0];
        console.log("single pro");
      })
      .catch(error => error);
  }
};
</script>

<style scoped>
.main-container {
  width: 80%;
  margin: 40px auto;
  padding: 20px;
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  padding: 0 0 150px 0;
}
.detail-img {
  margin-top: 100px;
  width: 450px;
  height: 300px;
}
.main-container img {
  width: 100%;
  height: 100%;
}
.detail-base {
  width: 40%;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.class-color {
  color: orange;
}
.class-size {
  font-size: 30px;
}
.mrp {
  font-size: 30px;
}
.display-color-outline {
  width: 60px;
  height: 60px;
  border: 2px solid grey;
  border-radius: 50px;
}
.display-color {
  width: 50px;
  height: 50px;
  background-color: black;
  border-radius: 50px;
  margin: 3px;
}
.product-color {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-weight: 600;
}
.details {
  text-align: justify;
}
@media screen and (max-width: 1200px) {
  .main-container {
    width: 90%;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
  }
  .detail-img {
    width: 50%;
  }
  .detail-base {
    margin-top: 30px;
  }
}
@media screen and (max-width: 950px) {
  .main-container {
    width: 90%;
    justify-content: center;
  }
  .detail-img {
    width: 60%;
  }
  .detail-img img {
    width: 100%;
  }
  .detail-base {
    width: 100%;
  }
}
@media screen and (max-width: 500px) {
  .main-container {
    width: 95%;
    justify-content: center;
  }
  .detail-img {
    width: 100%;
  }
  .detail-img img {
    width: 100%;
  }
  .detail-base {
    width: 100%;
  }
}
</style>