<template>
  <v-container>
    <v-row>
      <v-col cols="7">
        <v-card flat>
          <v-card-title>
            <span class="display-1">Thông tin đơn hàng</span>
          </v-card-title>
          <br />
          <v-row no-gutters justify="center">
            <v-col cols="12" md="3">
              <v-subheader style="font-size: 1.1rem">
                <b>Người nhận</b>
              </v-subheader>
            </v-col>
            <v-col cols="12" md="8">
              <ValidationProvider
                name="name"
                rules="required|alpha"
                v-slot="{ errors }"
                :bails="false"
              >
                <v-text-field label="Name" outlined v-model="name" counter="16"></v-text-field>
                <span class="red--text">{{ errors[0] }}</span>
              </ValidationProvider>
            </v-col>
          </v-row>

          <v-row no-gutters justify="center">
            <v-col cols="12" md="3">
              <v-subheader style="font-size: 1.1rem">
                <b>Địa chỉ</b>
              </v-subheader>
            </v-col>
            <v-col cols="12" md="8">
              <ValidationProvider
                name="address"
                rules="required|alpha"
                v-slot="{ errors }"
                :bails="false"
              >
                <v-text-field label="Address" outlined v-model="address" counter="100"></v-text-field>
                <span class="red--text">{{ errors[0] }}</span>
              </ValidationProvider>
            </v-col>
          </v-row>

          <v-row no-gutters justify="center">
            <v-col cols="12" md="3">
              <v-subheader style="font-size: 1.1rem">
                <b>Số điện thoại</b>
              </v-subheader>
            </v-col>
            <v-col cols="12" md="8">
              <ValidationProvider
                name="phone number"
                :rules="{required: true, regex:/^(0|\+84)[0-9]{9}$/}"
                v-slot="{ errors }"
                :bails="false"
              >
                <v-text-field label="Phone number" outlined v-model="phone"></v-text-field>
                <span class="red--text">{{ errors[0] }}</span>
              </ValidationProvider>
            </v-col>
          </v-row>

          <v-row no-gutters justify="center">
            <v-col cols="12" md="3">
              <v-subheader style="font-size: 1.1rem">
                <b>Ghi chú</b>
              </v-subheader>
            </v-col>
            <v-col cols="12" md="8">
              <ValidationProvider name="note" v-slot="{ errors }" :bails="false">
                <v-textarea label="Note" outlined v-model="note" counter="200"></v-textarea>
                <span class="red--text">{{ errors[0] }}</span>
              </ValidationProvider>
            </v-col>
          </v-row>
          <v-col cols="12" md="8" class="py-0">
            <v-subheader class="body-1">
              <p class="mb-0" style="color: red">Thanh toán khi nhận hàng</p>
            </v-subheader>
          </v-col>
          <v-card-actions>
            <v-btn class="mb-3 mx-auto" color="red" tile dark @click="order">Đặt hàng</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
      <v-col cols="5">
        <v-card flat>
          <v-card-title>
            <span class="display-1">Đơn hàng của bạn</span>
          </v-card-title>
          <v-simple-table class="table table-hover table-sm">
            <thead>
              <!-- <tr>
                <th class="text-center" style="width:30%"></th>
                <th class="text-center" style="width:50%"></th>
                <th class="text-center" style="width:20%"></th>
            
              </tr>-->
            </thead>
            <tbody>
              <tr v-for="product in products" :key="product.id">
                <td class="center">
                  <v-img max-width="80px" max-height="80px" :src="product.image"></v-img>
                </td>
                <td class="text-center">
                  {{ product.title }}
                  <br />
                  <b>x{{ product.quantity }}</b>
                </td>

                <td class="text-end">{{ formatCurrency(product.price) }}đ</td>
              </tr>
            </tbody>
            <tfoot>
              <td colspan="2" class="text-end">
                <v-card-text class="mx-auto color:white">
                  <label>
                    <b>Tổng tiền:</b>
                  </label>
                </v-card-text>
              </td>
              <td class="text-end">
                <span style="color: red" data-tongtien>{{ total }}đ</span>
              </td>
            </tfoot>
          </v-simple-table>
        </v-card>
      </v-col>
      <v-col></v-col>
    </v-row>
  </v-container>
</template>

<script>
import { mapGetters, mapState, mapMutations } from "vuex";
import { formatCurrency } from "../_api/format-currency";
export default {
  data: () => ({
    valid: true,
    name: "",

    address: "",

    phone: "",

    note: ""
  }),
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
      this.products.forEach(product => {
        numberProduct += product.quantity;
      });
      return numberProduct;
    }
  },
  methods: {
    validate() {
      if (this.$refs.form.validate()) {
        this.snackbar = true;
      }
    },
    formatCurrency(total) {
      return formatCurrency(total);
    }
    // reset() {
    //   this.$refs.form.reset();
    // }
  }
};
</script>