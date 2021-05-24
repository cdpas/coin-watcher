# Simple coin watcher for Android

This is a Vue-Cordova android app to track cryptocurrencies.
You can add the coins you have and the quantity to watch them without needing to check the wallet.
The prices are taken from Coingecko API.

The App had a button to Add coins where all Coingecko supported Coins and Token are listed.

The Settings windows (cogwheel) allows to download the stored data in a json file which then can be imported in order to have a backup.

The added coins (or tokens) can be edited by updating the quantity or deleted.

All data is stored in localStorage on the device.

[Download](https://cdpas.net/apk/coin-watcher.apk)



## Project setup
```
npm install
```

### Run the project in Vue
```
npm run serve
```

### Run on Android
```
npm run cordova-prepare 
cd src-cordova
cordova run android
```

### Download APK file for Android
```
The compiled APK for android can be downloaded from:
https://github.com/cdpas/coin-watcher/tree/master/release/coin-watcher.apk

```


