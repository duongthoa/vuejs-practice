<template>
  <v-data-table
    :headers="headers"
    :items="users"
    :sort-by="['userId']"
    user-key="userId"
    :sort-asc="[true]"
    :search="search"
  >
    <template v-slot:top>
      <v-toolbar flat color="white">
        <v-toolbar-title>CRUD Users</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <v-spacer></v-spacer>
        <v-spacer></v-spacer>
        <v-spacer></v-spacer>
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="Search with userId"
          color="it-blue-lighten"
          single-line
          hide-details
        ></v-text-field>
      </v-toolbar>
    </template>
    <template v-slot:item.action="{ item }">
      <v-icon small @click="deleteItem(item)" color="gg-red">mdi-trash-can-outline</v-icon>
    </template>
  </v-data-table>
</template>

<script>
export default {
  data: () => ({
    dialog: false,
    search: "",
    headers: [
      {
        text: "User Id",
        value: "userId",
        sortable: false,
        filterable: true
      },
      {
        text: "Name",
        value: "name",
        sortable: true,
        filterable: false
      },
      { text: "Email", value: "email", sortable: false, filterable: false },
      { text: "Phone", value: "phone", sortable: false, filterable: false },
      { text: "Address", value: "address", sortable: false, filterable: false },

      {
        text: "Birthday",
        value: "birthday",
        align: "center",
        sortable: true,
        filterable: false
      },
      { text: "Actions", align: "center", value: "action", filterable: false }
    ],
    users: [
      {
        userId: 1,
        name: "Le Thanh",
        email: "lethanh98@gmail.com",
        phone: "0123456789",
        address: "Truong Dinh, Hai Ba Trung",
        birthday: "26/06/1998"
      },
      {
        userId: 2,
        name: "Duong Thoa",
        email: "duongthoa98@gmail.com",
        phone: "0123445566",
        address: "Dai La, Hai Ba Trung",
        birthday: "04/11/1998"
      },
      {
        userId: 3,
        name: "Doan Dat",
        email: "doandat98@gmail.com",
        phone: "0336221717",
        address: "Phap Van",
        birthday: "28/04/1998"
      }
    ]
  }),

  methods: {
    deleteItem(item) {
      const index = this.users.indexOf(item);
      confirm("Are you sure you want to delete this user?") &&
        this.users.splice(index, 1);
    }
  }
};
</script>