<template>
  <v-menu open-on-hover bottom offset-y>
    <template v-slot:activator="{ on }">
      <v-btn icon to="/shopping-cart" exact v-on="on">
        <v-badge overlap color="gg-red">
          <template v-slot:badge>
            <span v-if="numberProduct > 0">{{ numberProduct }}</span>
          </template>
          <v-icon>mdi-cart-plus</v-icon>
        </v-badge>
      </v-btn>
    </template>
    <v-card v-if="numberProduct > 0" :width="450">
      <v-container>
        <v-row v-for="product in products" :key="product.id">
          <v-col cols="3">
            <v-img :height="60" contain :src="product.image"></v-img>
          </v-col>
          <v-col cols="5">{{ product.title }}x{{ product.quantity }}</v-col>
          <v-col cols="4">{{ formatCurrency(product.price) }}đ</v-col>
        </v-row>
        <v-row>
          <v-col cols="6"></v-col>
          <v-col cols="6">Tổng tiền: {{ total }}đ</v-col>
        </v-row>
        <v-card-actions>
          <v-btn
            color="gg-red"
            class="ma-2"
            outlined
            :disabled="!products.length"
            @click="cartdetail"
          >Xem chi tiết</v-btn>
          <v-btn
            color="gg-red"
            tile
            dark
            class="ma-2"
            :disabled="!products.length"
            @click="goToOrderPage"
          >Đặt hàng ngay</v-btn>
        </v-card-actions>
        <p v-show="checkoutStatus">Checkout {{ checkoutStatus }}.</p>
      </v-container>
    </v-card>

    <v-card v-else :width="450">
      <v-img
        class="white--text mx-auto"
        max-width="250"
        max-height="200"
        :src="require('@/_assets/image/empty-cart.png')"
      ></v-img>
    </v-card>
  </v-menu>
</template>
<script>
import { mapGetters, mapState, mapMutations } from "vuex";
import { formatCurrency } from "../_api/format-currency";

export default {
  data() {
    return {
      dialog: false
    };
  },
  computed: {
    ...mapState({
      checkoutStatus: state => state.cart.checkoutStatus
    }),
    ...mapGetters("cart", {
      products: "cartProducts",
      total: "cartTotalPrice"
    }),
    numberProduct: function() {
      let numberProduct = 0;
      if (this.products.length > 0) {
        this.products.forEach(product => {
          numberProduct += product.quantity;
        });
      }
      return numberProduct;
    }
  },
  methods: {
    goToOrderPage() {
      const path = this.$route.path;
      this.dialog = false;
      if (path !== "/order-page") {
        this.$router.push({ name: "order page" });
      }
    },
    cartdetail() {
      const path = this.$route.path;
      this.dialog = false;
      if (path !== "/shopping-cart") {
        this.$router.push({ name: "shopping cart" });
      }
    },

    ...mapMutations("cart", [
      "incrementItemQuantity",
      "decrementItemQuantity",
      "removeProductFromCart"
    ]),
    formatCurrency(price) {
      return formatCurrency(price);
    },
    onMouseOver() {
      this.dialog = true;
    },
    onMouseOut() {
      this.dialog = false;
    }
  }
};
</script>

<style scoped>
v-btn-tpggle v-btn {
  min-width: 2rem;
}
</style>
