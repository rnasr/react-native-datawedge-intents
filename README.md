*Please be aware that this application / sample is provided as-is for demonstration purposes without any guarantee of support*
=========================================================

*NOTE: I forked from darryncampbell/react-native-datawedge-intents to add support for Andrdoid 12+ that requires BroadcastReceiver export status to be explicitly declared.*


# React-Native-DataWedge-Intents
React Native Android module to interface with Zebra's DataWedge Intent API

[![npm version](http://img.shields.io/npm/v/react-native-datawedge-intents.svg?style=flat-square)](https://npmjs.org/package/react-native-datawedge-intents "View this project on npm")
[![npm downloads](http://img.shields.io/npm/dm/react-native-datawedge-intents.svg?style=flat-square)](https://npmjs.org/package/react-native-datawedge-intents "View this project on npm")
[![npm downloads](http://img.shields.io/npm/dt/react-native-datawedge-intents.svg?style=flat-square)](https://npmjs.org/package/react-native-datawedge-intents "View this project on npm")
[![npm licence](http://img.shields.io/npm/l/react-native-datawedge-intents.svg?style=flat-square)](https://npmjs.org/package/react-native-datawedge-intents "View this project on npm")

This module is useful when developing React Native applications for Zebra mobile computers, making use of the Barcode Scanner

### Installation

```bash
npm install react-native-datawedge-intent-rnasr --save
```

## Example usage

```javascript
import DataWedgeIntents from 'react-native-datawedge-intent-rnasr'
...
//  Register a receiver for the barcode scans with the appropriate action
DataWedgeIntents.registerReceiver('com.zebra.dwintents.ACTION', '');
...
//  Declare a handler for barcode scans
this.scanHandler = (deviceEvent) => {console.log(deviceEvent);};
...
//  Listen for scan events sent from the module
DeviceEventEmitter.addListener('barcode_scan', this.scanHandler);
...
//  Initiate a scan (you could also press the trigger key)
DataWedgeIntents.sendIntent(DataWedgeIntents.ACTION_SOFTSCANTRIGGER,DataWedgeIntents.START_SCANNING);

```

```javascript
import DataWedgeIntents from 'react-native-datawedge-intents'
...
//  Register a receiver for the barcode scans with the appropriate action
DataWedgeIntents.registerBroadcastReceiver({
  filterActions: [
      'com.zebra.reactnativedemo.ACTION',
      'com.symbol.datawedge.api.RESULT_ACTION'
  ],
  filterCategories: [
      'android.intent.category.DEFAULT'
  ]
});
...
//  Declare a handler for broadcast intents
this.broadcastReceiverHandler = (intent) =>
{
  this.broadcastReceiver(intent);
}
...
//  Initiate a scan (you could also press the trigger key)
this.sendCommand("com.symbol.datawedge.api.SOFT_SCAN_TRIGGER", 'TOGGLE_SCANNING');
...
sendCommand(extraName, extraValue) {
  console.log("Sending Command: " + extraName + ", " + JSON.stringify(extraValue));
  var broadcastExtras = {};
  broadcastExtras[extraName] = extraValue;
  broadcastExtras["SEND_RESULT"] = this.sendCommandResult;
  DataWedgeIntents.sendBroadcastWithExtras({
    action: "com.symbol.datawedge.api.ACTION",
    extras: broadcastExtras});
}
```

## DataWedge

This module **requires the DataWedge service running** on the target device to be **correctly configured** to broadcast Android intents on each barcode scan with the appropriate action.  This can be achieved either manually or via an API, see the sample application readme files for a more thorough explanation.

### Output Plugin

Please also ensure you disable the keyboard output plugin to avoid undesired effects on your application: [thread](https://developer.zebra.com/message/95397).

For more information about DataWedge and how to configure it please visit Zebra [tech docs](http://techdocs.zebra.com/).  The DataWedge API that this module calls is detailed [here](http://techdocs.zebra.com/datawedge/latest/guide/api/)


