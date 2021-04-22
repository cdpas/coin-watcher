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
          @click="getSingleCoin()"
          style="color: #006d77"
          >mdi-file-edit</v-icon>
      </template>
      <v-card style="background-color: #edf6f9">
        <v-card-title><h4 style="width:100%;text-align:center">Edit Coin to Watchlist</h4> </v-card-title>
        <v-divider></v-divider>
        <v-card-text style="height: 300px;">
            <br>
           <p style="font-weight: bold;">{{this.myCoin.name}} {{this.myCoin.symbol}}</p>
            <h3>Quantity:</h3>
            <v-text-field
            v-model="quantity"
            :label="this.myCoin.quantity"
            type="number"
            placeholder="0"
            outlined
          ></v-text-field>
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
            @click="updateCoin()"
          >
            Update
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
    props: ['id'],
    name: 'Home',

    data: () => ({
      dialog: false,
      myCoins: [],
      coin: '',
      quantity: '',
      myCoin: []
    }),
    methods: {
      getSingleCoin() {
      this.myCoins = JSON.parse(localStorage.getItem("myCoins"));
      this.myCoin = this.myCoins.find(list => list.id == this.id);
      this.availableCoins = JSON.parse(localStorage.getItem("coingeckoList"));   
      console.log(this.myCoin)
      },
      updateCoin(){
        this.myCoins[this.id].quantity = this.quantity;
        localStorage.setItem("myCoins", JSON.stringify(this.myCoins));
        console.log(this.myCoins);
        this.$emit('added-new-coin');
        this.dialog = false;
      }
    },
    mounted() {
    }
  }
</script>