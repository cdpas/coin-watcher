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
          style="color: #006d77; position:absolute; right: 10px"
          >mdi-cog</v-icon>
      </template>
      <v-card style="background-color: #edf6f9">
        <v-card-title>Settings</v-card-title>
        <v-divider></v-divider>
        <v-card-text style="height: 300px;">
            <br>
            <div style="text-align:center">
              <h3>Download Backup</h3>
              <br>
              <v-btn @click="download()"
              color="#006d77"
              style="color: white"
              >Download</v-btn>
              <br><br>
              <h3>Import Backup file</h3><br>
              <input type="file" @change="readFile($event)">
              <div v-if="this.dataUploaded"> <br>
              <v-btn
              @click="importData()"
              color="#006d77"
              style="color: white"
              >
                Import
              </v-btn>
              </div>
            </div>
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
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>
<style scoped>
</style>
<script>
var FileSaver = require('file-saver'); 
  export default {
    name: 'Settings',
    components: {
    },
    data: () => ({
      dialog: false,
      snackbar: false,
      snackText: '',
      myCoins: [],
      importedFile: '',
      dataUploaded: false
    }),
    methods: {
     checkMyCoins() {
       if (localStorage.getItem("myCoins")) {
          var local = localStorage.getItem("myCoins");
          if (local.length > 0) {
            this.myCoins = JSON.parse(localStorage.getItem("myCoins"));
          } 
        } 
     },
      download() {
        this.myCoins = JSON.parse(localStorage.getItem("myCoins"));
        var file = new File([JSON.stringify(this.myCoins)], "backup.json", {type: "text/plain;charset=utf-8"});
        FileSaver.saveAs(file);
      },
      readFile(event) {
        var file = event.target.files[0];
        var reader = new FileReader();
        reader.onload = (e) => {
          this.importedFile = e.target.result;
        }
        reader.readAsText(file);
        this.dataUploaded = true;
      },
      importData() {
        localStorage.setItem("myCoins", this.importedFile);
        this.$emit('imported')
        this.dialog = false;
      }
    },
    mounted() {
      this.checkMyCoins();
    }
  }
</script>