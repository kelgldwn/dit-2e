<template>
  <v-row justify="center" align="center">
    <v-col cols="12">
      <v-data-table
    :headers="headers"
    :items="customers"
    :items-per-page="10"
    class="elevation-1"
    :loading="loading"
    loading-text="Loading...Please wait"
  >
  <template v-slot:item.profilePicture="{ item }">
   <v-img
  lazy-src="https://picsum.photos/id/11/10/6"
  max-height="80"
  max-width="80"
  :src="item.profilePicture"
></v-img>
 <v-badge 
          color="blue"
          content="VIP"
          v-if="item.isVip == true"
          offset-x=""
        ></v-badge>
        <v-avatar size="60">  
        
        </v-avatar>

      </template>
       <template v-slot:item.actions="{ item }">
        <v-btn color="primary" @click="getCustomersDetails(item)">View</v-btn>
       </template>
    </v-data-table>

    <v-dialog v-model="customerDialog" width="500">
      
      <v-card>
        <v-card-title class="text-h5 purple"><v-icon>mdi-information</v-icon> <v-spacer></v-spacer>
        Customer Details <v-spacer></v-spacer>
      <v-btn color="pink"> <v-icon>mdi-close</v-icon></v-btn>
    </v-card-title>
    <v-spacer></v-spacer>

      </v-card>
        <v-card-actions> </v-card-actions>  
        <v-card-text>Name</v-card-text>
        <v-card-actions></v-card-actions>
        <v-card-text>Email</v-card-text>
        <v-card-actions> </v-card-actions>
        <v-card-text>Phone #</v-card-text>
        <v-card-actions> </v-card-actions>
        <v-card-text>Age</v-card-text>
        <v-card-actions> </v-card-actions>

        
      </v-card>
    </v-dialog>
  </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return{
      headers: [
          {

          },
          { text: 'Photo', value: 'profilePicture' },
          { text: 'Name', value: 'name ' },
          { text: 'Email', value: 'email' },
          { text: 'Phone', value: 'phone' },
          { text: 'Address', value: 'protein' },
          { text: 'Age', value: 'age'},
          { text: 'Status', value: 'isVip'},
          { text: "", value: 'actions'},
        ],
        customers: [],
        loading: true,
        customerDialog: false,
        customerName: "",
        customerEmail: "",
        customerPhone: "",
        customerAge: 0,
        customerPhoto: ""
    }
  },
  methods: {
    getCustomers() {
      this.$axios.get('/customers')
      .then(response => {
        console.log(response.data)
        this.customers = response.data;
        this.loading = false
      })
      .catch(err => {
        console.log(err)
      })
    },

    getCustomersDetails(item) {
      console.log(item)

    this.customerName = item.Name;
    this.customerEmail = item.Email;
    this.customerAge = item.Age;
    this.customerPhone = item.Phone;
    this.customerPhoto = item.profilePicture; 
    this.customerDialog = true;
    }
  },

  mounted() {
    this.getCustomers()
  }
}
</script>