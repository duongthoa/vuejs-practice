<template>
  <v-container class="px-0">
    <!-- <ValidationObserver ref="filterProduct"> -->
    <!-- <v-btn-toggle> -->
    <v-row no-gutters justify="space-around">
      <v-col cols="5">
        <v-btn @click="resetFilter" depressed outlined color="gg-red">Xóa bộ lọc</v-btn>
      </v-col>

      <v-col cols="5">
        <v-btn
          depressed
          color="gg-red"
          class="white--text"
          :loading="loading"
          :disabled="loading"
          @click="loader"
          width="100%"
        >Áp dụng</v-btn>
      </v-col>
    </v-row>
    <!-- <v-btn :loading="loading" :disabled="loading" @click="loader = 'loading'">Accept Terms 2</v-btn> -->
    <!-- </v-btn-toggle> -->
    <!-- <v-container fluid> -->
    <v-list>
      <template v-for="item in items">
        <v-list-group no-action :key="item.text">
          <template v-slot:activator>
            <v-list-item-content>
              <v-list-item-title>{{ item.text }}</v-list-item-title>
            </v-list-item-content>
          </template>
          <v-list shaped>
            <v-list-item-group v-model="filter[item.model]" multiple>
              <template v-for="(child, i) in item.children">
                <v-divider v-if="!child" :key="`divider-${i}`"></v-divider>
                <v-list-item
                  v-else
                  :key="`child.text-${i}`"
                  :value="child.text"
                  active-class="deep-purple--text text--accent-4"
                >
                  <template v-slot:default="{ active, toggle }">
                    <v-list-item-content>
                      <v-list-item-title v-text="child.text"></v-list-item-title>
                    </v-list-item-content>
                    <v-list-item-action>
                      <v-checkbox
                        :input-value="active"
                        :true-value="child.text"
                        color="deep-purple accent-4"
                        @click="toggle"
                      ></v-checkbox>
                    </v-list-item-action>
                  </template>
                </v-list-item>
              </template>
            </v-list-item-group>
          </v-list>
        </v-list-group>
      </template>
    </v-list>
    <!-- </v-container> -->
    <!-- </ValidationObserver> -->
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      filter: {
        price: [],
        brand: [],
        color: [],
        ROM: [],
        frontCamera: [],
        backCamera: [],
        system: [],
        RAM: []
      },
      //   toggle_exclusive: undefined,
      //   loader: null,
      loading: false,
      items: [
        {
          text: "Giá",
          model: "price",
          children: [
            { text: "Dưới 5.000.000" },
            { text: "5.000.000 - 10.000.000" },
            { text: "10.000.000 - 20.000.000" },
            { text: "20.000.000 - 30.000.000" },
            { text: "Trên 30.000.000" }
          ]
        },
        {
          text: "Thương hiệu",
          model: "brand",
          children: [
            { text: "Sam Sung" },
            { text: "iPhone" },
            { text: "Xiaomi" },
            { text: "Oppo" },
            { text: "Realme" },
            { text: "Huawei" },
            { text: "NOKIA" }
          ]
        },
        {
          text: "Màu sắc",
          model: "color",
          children: [
            { text: "Đen" },
            { text: "Xanh lam" },
            { text: "Xanh lục" },
            { text: "Vàng" },
            { text: "Đỏ" },
            { text: "Trắng" },
            { text: "Bạc" },
            { text: "Xám" },
            { text: "Tím" },
            { text: "Hồng" },
            { text: "Cam" },
            { text: "Đồng" },
            { text: "Xanh ngọc" }
          ]
        },
        {
          text: "Bộ nhớ trong",
          model: "ROM",
          children: [
            { text: "16GB" },
            { text: "32GB" },
            { text: "64GB" },
            { text: "128GB" },
            { text: "256GB" },
            { text: "512GB" }
          ]
        },
        {
          text: "Camera sau",
          model: "backCamera",
          children: [
            { text: "2MP" },
            { text: "5MP" },
            { text: "8MP" },
            { text: "12MP" },
            { text: "13MP" },
            { text: "2x12MP" },
            { text: "3x12MP" },
            { text: "16MP" },
            { text: "24MP" },
            { text: "25MP" },
            { text: "32MP" },
            { text: "48MP" },
            { text: "64MP" }
          ]
        },
        {
          text: "Camera trước",
          model: "frontCamera",
          children: [
            { text: "2MP" },
            { text: "5MP" },
            { text: "7MP" },
            { text: "8MP" },
            { text: "12MP" },
            { text: "13MP" },
            { text: "16MP" },
            { text: "20MP" },
            { text: "24MP" },
            { text: "25MP" },
            { text: "48MP" },
            { text: "TOF 3D" }
          ]
        },
        {
          text: "Hệ điều hành",
          model: "system",
          children: [
            { text: "Android 7.0" },
            { text: "Android 8.1" },
            { text: "Androi 9.0" },
            { text: "iOS 12" },
            { text: "iOS 13" }
          ]
        },
        {
          text: "RAM",
          model: "RAM",
          children: [
            { text: "2GB" },
            { text: "3GB" },
            { text: "4GB" },
            { text: "6GB" },
            { text: "8GB" },
            { text: "12GB" }
          ]
        }
      ]
    };
  },
  //   watch: {
  //     loader() {
  //       const l = this.loader;
  //       this[l] = !this[l];
  //       setTimeout(() => (this[l] = false), 3000);

  //       this.loader = null;
  //     }
  //   },
  // watch: {
  //   menu() {
  //     this.$refs.form.reset();
  //   }
  // },
  methods: {
    loader() {
      this.loading = true;
      setTimeout(() => (this.loading = false), 1000);
    },
    resetFilter() {
      this.filter.price = [];
      this.filter.brand = [];
      this.filter.color = [];
      this.filter.ROM = [];
      this.filter.frontCamera = [];
      this.filter.backCamera = [];
      this.filter.system = [];
      this.filter.RAM = [];
    }
  }
};
</script>

<style lang="scss" scoped>
@import "@/style.scss";
.container {
  background-color: $it-bg-in;
}
</style>