<template>
  <div class="checkout-box">
    <ul class="checkout-list">
      <transition-group name="fade">
        <li
          v-for="(product, index) in getProductsInCart"
          :key="index"
          class="checkout-product"
        >
          <img :src="product.image" alt="" class="product-image" />
          <h3 class="product-name">{{ product.name }}</h3>
          <span class="product-price">R$ {{ product.price }},00 </span>
          <button class="product-remove" @click="remove(index);">X</button>
        </li>
      </transition-group>
    </ul>
    <div v-if="!hasProduct()" class="checkout-message">
      <h3>No products...</h3>
      <router-link to="./">Back to list of products</router-link>
    </div>
    <h3 class="total" v-if="hasProduct()">Total: R$ {{ totalPrice() }}, 00</h3>
    <div v-show="true" class="text-xs-center">
      <v-dialog v-model="dialog" width="500">
        <v-btn slot="activator" color="red lighten-2" dark>
          Save and Continue
        </v-btn>
        <v-card>
          <v-card-title class="headline grey lighten-2" primary-title>
            Payment Via Cash on delivery
          </v-card-title>

          <v-card-text> Purchase Successful </v-card-text>

          <v-divider></v-divider>

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary" flat @click="dialog = false;">
              Thanks for shopping!
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>
    <div v-show="false" class="text-xs-center">
      <v-dialog v-model="dialog" width="500">
        <v-btn slot="activator" color="red lighten-2" dark>
          Save and Continue
        </v-btn>
        <v-card>
          <v-card-title class="headline grey lighten-2" primary-title>
            Login Issue
          </v-card-title>

          <v-card-text> Sign In Error! </v-card-text>

          <v-divider></v-divider>

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary" flat @click="dialog = false;">
              cancel
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from "vuex";
import signuperror from "./Modal.vue";
export default {
  components: [signuperror],
  computed: {
    ...mapGetters(["getProductsInCart"])
  },

  methods: {
    ...mapActions(["removeProduct"]),
    hasProduct() {
      return this.getProductsInCart.length > 0;
    },
    totalPrice() {
      return this.getProductsInCart.reduce(
        (current, next) => current + next.price,
        0
      );
    },
    remove(index) {
      this.removeProduct(index);
    }
  }
};
</script>

<style scoped>
.checkout-box {
  width: 100%;
  max-width: 900px;
  display: flex;
  flex-direction: column;
  margin: 50px auto;
  box-sizing: border-box;
  padding: 1em;
}

.checkout-list {
  padding: 0;
}

.checkout-product {
  display: grid;
  grid-template-columns: 1fr 3fr 2fr 0.5fr;
  background-color: #fff;
  box-shadow: 0px 0px 10px rgba(73, 74, 78, 0.1);
  border-radius: 5px;
  list-style: none;
  box-sizing: border-box;
  padding: 0.8em;
  margin: 1em 0;
}

.checkout-product * {
  place-self: center;
}
.product-image {
  grid-column: 1/2;
  width: 50%;
}

.product-name {
  box-sizing: border-box;
}

.product-price {
  font-size: 1.2em;
  font-weight: bold;
}

.product-remove {
  width: 25px;
  height: 25px;
  border-radius: 50%;
  border: 0;
  background-color: #e0e0e0;
  color: #fff;
  cursor: pointer;
}

.total {
  font-size: 2em;
  font-weight: bold;
  align-self: flex-end;
}

.checkout-message {
  font-size: 1.5em;
}

.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s;
}

.fade-enter,
.fade-leave-to {
  transform: translateX(-40px);
  opacity: 0;
}
</style>
