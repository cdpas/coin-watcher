<template>
  <v-container style="background-color: #edf6f9; min-height: calc(100vh - 56px)">
      <div style="max-width: 100vw; text-align: center; margin-top: 25px;">
        <Settings @imported="checkMyCoins"></Settings>
        <AddCoin @added-new-coin="checkMyCoins"></AddCoin>
      </div>
      <div>
      </div>
      <div style="margin-top: 25px; margin-bottom: 25px">
        <thead>
        <tr>
          <th scope="col" style="width: 20vw">Coin</th>
          <th scope="col" style="width: 20vw">Quantity</th>
          <th scope="col" style="width: 40vw">Value</th>
          <th scope="col" style="width: 10vw"></th>
          <th scope="col" style="width: 10vw"></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(coin, index) in myCoins" :key="index" style="text-align: center;">
          <td class="exchange fontsize">{{ coin.symbol }}</td>
          <td class="date fontsize">{{ coin.quantity }}</td>
          <td class="quantity fontsize valueCoin">{{currentCoinValue(coin.coinid, coin.quantity)}} €</td>
          <td>
            <EditCoin :id="coin.id" @added-new-coin="checkMyCoins"></EditCoin>
            </td>
          <td>
            <DeleteCoin :index="index" :symbol="coin.symbol" @added-new-coin="checkMyCoins"></DeleteCoin>
           </td>
        </tr>
      </tbody>
      </div>
    <v-snackbar
      v-model="snackbar"
      timeout="3000"
    >
      {{ snackText }}
      <template v-slot:action="{ attrs }">
        <v-btn
          color="pink"
          text
          v-bind="attrs"
          @click="snackbar = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </v-container>
</template>
<style scoped>
td {
  height: 40px;
}
tr:nth-child(even) {
  background-color: #83c5be;
}
.fontsize {
  font-size: 14px;
}
</style>
<script>
import AddCoin from "@/components/add.vue";
import EditCoin from "@/components/edit.vue";
import DeleteCoin from "@/components/delete.vue";
import Settings from "@/components/settings.vue";
const axios = require('axios');
  export default {
    name: 'Home',
    components: {
      AddCoin,
      EditCoin,
      DeleteCoin,
      Settings
    },
    data: () => ({
      snackbar: false,
      snackText: '',
      myCoins: [],
      newCoin: 0,
      onlyCoinId: '',
      currentValue: [],
      myFilename: ''
    }),
    methods: {
     async getCoinsList() {
       await axios.get('https://api.coingecko.com/api/v3/coins/list?include_platform=false')
       .then((data) => {
         this.availableCoins = data.data;
        })
     },
     checkMyCoins() {
       if (localStorage.getItem("myCoins")) {
          var local = localStorage.getItem("myCoins");
          if (local.length > 0) {
            this.myCoins = JSON.parse(localStorage.getItem("myCoins"));
            this.onlyCoinId = '';
            var i;
            for (i = 0; i < this.myCoins.length; i++) {
              this.onlyCoinId += this.myCoins[i].coinid + ',';
            }
            this.getValue();
          } else {
          console.log("vuoto")
          } 
        } 
     },
     async getValue() {
       await axios.get(`https://api.coingecko.com/api/v3/simple/price?ids=${this.onlyCoinId}&vs_currencies=eur`)
        .then((data) => {
          this.currentValue = data;
        })
     },
      currentCoinValue(val1, val2) {
        var coin = this.currentValue.data.[val1].eur * val2;
        return coin.toFixed(2);
      },
    },
    mounted() {
      this.checkMyCoins();
    }
  }
</script>