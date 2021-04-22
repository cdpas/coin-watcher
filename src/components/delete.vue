<template>
  <v-row justify="center">
    <v-dialog
      v-model="dialog"
      scrollable
      max-width="300px"
    >
      <template v-slot:activator="{ on }">
          <v-icon
          v-on="on"
          style="color: #e29578"
          >mdi-delete</v-icon>
      </template>
      <v-card style="background-color: #edf6f9">
        <v-card-title>
          <h4 style="width:100%;text-align:center">
            Edit Coin to Watchlist
            </h4></v-card-title>
        <v-divider></v-divider>
        <v-card-text style="height: 100px; font-weight:bold">
            <br>
           Are you sure you want to delete {{symbol}}?
        </v-card-text>
        <v-divider></v-divider>
        <v-card-actions>
          <v-btn
            color="red darken-1"
            text
            @click="dialog = false"
          >
            Close
          </v-btn>
          <v-btn
            color="blue darken-1"
            style="right:10px!important; position: absolute;"
            text
            @click="remove(index)"
          >
            Delete
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>
<style scoped>
</style>
<script>
  export default {
    props: ['index','symbol'],
    name: 'Home',
    data: () => ({
      dialog: false,
      myCoins: [],
      coin: '',
      quantity: '',
      myCoin: []
    }),
    methods: {
      getMyCoins() {
      this.myCoins = JSON.parse(localStorage.getItem("myCoins"));
      },
      remove(value) {
           this.$delete(this.myCoins, value);
           console.log(this.myCoins);
           localStorage.setItem("myCoins", JSON.stringify(this.myCoins));
           this.$emit('added-new-coin');
          this.dialog = false;
      }
    },
    mounted() {
      this.getMyCoins();
    }
  }
</script>