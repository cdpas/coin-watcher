<template>
  <v-row justify="center">
    <v-dialog
      v-model="dialog"
      scrollable
      max-width="300px"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="#006d77"
          dark
          v-bind="attrs"
          v-on="on"
        >
          Add Coin
        </v-btn>
      </template>
      <v-card style="background-color: #edf6f9">
        <v-card-title><h4 style="width:100%;text-align:center">
          Add Coin to Watchlist
          </h4></v-card-title>
        <v-divider></v-divider>
        <v-card-text style="height: 300px;">
            <br>
           <h3>Select coin:</h3>
             <v-autocomplete
              v-model="coin"
              :items="availableCoins"
              dense
              filled
              label="Coins"
              item-text="symbol"
              item-value="id"
              return-object
            ></v-autocomplete>
            <h3>Quantity:</h3>
            <v-text-field
            v-model="quantity"
            label="Quantity"
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
            @click="saveCoin()"
          >
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>
<style scoped>
</style>
<script>
const axios = require('axios');
  export default {
    name: 'Home',
    data: () => ({
      availableCoins: [],
      dialog: false,
      myCoins: [],
      coin: '',
      quantity: ''
    }),
    methods: {
      checkTemp() {
        if (localStorage.getItem("coingeckoList") != null) {
         this.availableCoins = JSON.parse(localStorage.getItem("coingeckoList")); 
        } else {
          this.getCoinsList();
        }
      },
      loadMyCoins() {
        if (localStorage.getItem("myCoins")) {
          var local = localStorage.getItem("myCoins");
          if (local.length > 0) {
            this.myCoins = JSON.parse(localStorage.getItem("myCoins"));
          } 
        } else {
          localStorage.setItem("myCoins", this.myCoins);
        }
      },
     async getCoinsList() {
       await axios.get('https://api.coingecko.com/api/v3/coins/list?include_platform=false')
       .then((data) => {
         this.availableCoins = data.data;
         localStorage.setItem("coingeckoList", JSON.stringify(this.availableCoins));
         this.loadMyCoins();
        })
     },
     saveCoin(){
       if (localStorage.getItem("myCoins").length > 0) {
         this.myCoins = JSON.parse(localStorage.getItem("myCoins"));
       }
      var arrLen = this.myCoins.length;
        this.myCoins.push({id: (arrLen), coinid: this.coin.id, symbol: this.coin.symbol, name: this.coin.name, quantity: this.quantity});
        localStorage.setItem("myCoins", JSON.stringify(this.myCoins));
        this.$emit('added-new-coin');
        this.coin = '';
        this.quantity = '';
        this.dialog = false;
     }
    },
    mounted() {
      this.checkTemp();
      this.loadMyCoins();
    }
  }
</script>