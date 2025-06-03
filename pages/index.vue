<template>
  <v-row justify="center" align="center">
    <v-col cols="12">
      <v-data-table
    :headers="headers"
    :items="customers"
    :items-per-page="10"
    class="elevation-1"
    loading="loading"
    loading-text="Loading... Please wait"
  >

  <template v-slot:item.profilePicture="{item}">
  <v-img
  
  lazy-src="https://picsum.photos/id/11/10/6"
  max-height="80"
  max-width="80"
  :src="item.profilePicture"
  
><v-badge
        bordered
          offset-x="-50"
  offset-y="40"
        color="error"
        content="VIP"
        v-if="item.isVip == true"
        overlap
      ></v-badge></v-img>
  
</template>
</v-data-table>


    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      headers: [
         
          { text: 'Photo', value: 'profilePicture' },
          { text: 'Name', value: 'name' },
          { text: 'Email', value: 'email' },
          { text: 'Phone', value: 'phone' },
          { text: 'Address', value: 'address' },
          { text: 'Age', value: 'age' },
          { text: 'Status', value: 'isVip' },
        ],
        customers: [],
        loading: false
    }
  },
  methods: {
    getCustomers() {
      this.$axios.get('/customers')
      .then(response =>{
        console.log(response.data)
        this.customers = response.data
        this.loading = false
      })
      .catch(err => {
        console.log(err)
      })
    }
  },

  mounted() {
    this.getCustomers()
  }
}
</script>
