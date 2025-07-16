<template>
  <v-row justify="center" align="center">
    <v-col cols="12">
      <!-- Search -->
      <v-text-field
        v-model="search"
        label="Search customers"
        prepend-inner-icon="mdi-magnify"
        outlined
        dense
        class="mb-4"
      ></v-text-field>

      <!-- Data Table -->
      <v-data-table
        :headers="headers"
        :items="filteredCustomers"
        :items-per-page="itemsPerPage"
        class="elevation-1"
        :loading="loading"
        loading-text="Loading... Please wait"
      >
        <!-- Profile Picture -->
        <template v-slot:item.profilePicture="{ item }">
          <v-avatar size="60">
            <v-img
              :src="item.profilePicture"
              lazy-src="https://picsum.photos/id/11/10/6"
            ></v-img>
          </v-avatar>
          <v-badge v-if="item.isVip" color="blue" content="VIP" class="ml-2" />
        </template>

        <!-- Actions -->
        <template v-slot:item.actions="{ item }">
          <v-btn small color="primary" @click="getCustomersDetails(item)">
            <v-icon left>mdi-eye</v-icon> View
          </v-btn>
          <v-btn small color="orange" @click="openEditDialog(item)">
            <v-icon left>mdi-pencil</v-icon> Edit
          </v-btn>
          <v-btn small color="red" @click="deleteCustomer(item.id)">
            <v-icon left>mdi-delete</v-icon> Delete
          </v-btn>
        </template>
      </v-data-table>

      <!-- Customer Details Dialog -->
<v-dialog v-model="customerDialog" width="500">
  <v-card>
    <v-card-title class="justify-center">
      <v-avatar size="120">
        <v-img :src="customerPhoto" />
      </v-avatar>
    </v-card-title>

    <v-card-subtitle class="text-center mt-2">
      <strong>{{ customerName }}</strong>
    </v-card-subtitle>

    <v-card-text class="text-center">
      <p><strong>Email:</strong> {{ customerEmail }}</p>
      <p><strong>Phone:</strong> {{ customerPhone }}</p>
      <p><strong>Age:</strong> {{ customerAge }}</p>
    </v-card-text>

    <v-card-actions>
      <v-spacer></v-spacer>
      <v-btn color="primary" text @click="customerDialog = false">Close</v-btn>
    </v-card-actions>
  </v-card>
</v-dialog>

      <!-- Edit Customer Dialog -->
      <v-dialog v-model="editDialog" width="500">
        <v-card>
          <v-card-title>
            <v-icon class="mr-2">mdi-pencil</v-icon>
            Idit Customir
            <v-spacer></v-spacer>
            <v-btn icon @click="editDialog = false">
              <v-icon>mdi-close</v-icon>
            </v-btn>
          </v-card-title>
          <v-card-text>
            <v-text-field v-model="editForm.name" label="Name" outlined />
            <v-text-field v-model="editForm.email" label="Email" outlined />
            <v-text-field v-model="editForm.phone" label="Phone" outlined />
            <v-text-field
              v-model="editForm.age"
              label="Age"
              outlined
              type="number"
            />
            <v-switch v-model="editForm.isVip" label="VIP Customer" />
          </v-card-text>
          <v-card-actions>
            <v-btn color="primary" @click="saveEdit">Save</v-btn>
            <v-spacer></v-spacer>
            <v-btn text @click="editDialog = false">Cancel</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: "IndexPage",
  data() {
    return {
      headers: [
        { text: "Photo", value: "profilePicture" },
        { text: "Name", value: "name" },
        { text: "Email", value: "email" },
        { text: "Phone", value: "phone" },
        { text: "Address", value: "address" },
        { text: "Age", value: "age" },
        { text: "Status", value: "isVip" },
        { text: "Actions", value: "actions", sortable: false },
      ],
      customers: [],
      loading: true,
      search: "",
      itemsPerPage: 10,

      customerDialog: false,
      customerName: "",
      customerEmail: "",
      customerPhone: "",
      customerAge: 0,
      customerPhoto: "",

      editDialog: false,
      editForm: {
        id: null,
        name: "",
        email: "",
        phone: "",
        age: 0,
        isVip: false,
      },
    };
  },
  computed: {
    filteredCustomers() {
      if (!this.search) return this.customers;
      const term = this.search.toLowerCase();
      return this.customers.filter(
        (c) =>
          c.name.toLowerCase().includes(term) ||
          c.email.toLowerCase().includes(term) ||
          c.phone.toLowerCase().includes(term)
      );
    },
  },
  methods: {
    getCustomers() {
      this.$axios
        .get("/customers")
        .then((res) => {
          this.customers = res.data;
          this.loading = false;
        })
        .catch((err) => {
          console.error(err);
          this.loading = false;
        });
    },
    getCustomersDetails(item) {
      this.customerName = item.name;
      this.customerEmail = item.email;
      this.customerPhone = item.phone;
      this.customerAge = item.age;
      this.customerPhoto = item.profilePicture;
      this.customerDialog = true;
    },
    openEditDialog(item) {
      this.editForm = { ...item }; // Pre-fill form
      this.editDialog = true;
    },
    saveEdit() {
      this.$axios
        .put(`/customers/${this.editForm.id}`, this.editForm)
        .then(() => {
          alert("Nigger updated successfully!");
          this.editDialog = false;
          this.getCustomers(); // refresh list
        })
        .catch((err) => {
          console.error(err);
          alert("Error updating customer.");
        });
    },
    deleteCustomer(id) {
      if (confirm("Nigger are you sure about this shi?")) {
        this.$axios
          .delete(`/customers/${id}`)
          .then(() => {
            alert("Nigger deleted successfully!");
            this.getCustomers();
          })
          .catch((err) => {
            console.error(err);
            alert("Error deleting Nigger.");
          });
      }
    },
  },
  mounted() {
    this.getCustomers();
  },
};
</script>
