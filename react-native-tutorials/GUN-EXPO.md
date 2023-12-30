_this is tested on windows host , building for android_

# prereqs:
  * android simulator
  * nodejs

# step1: expo bare tempalte

```
npx create-expo-app my-app
```

# step2: install packages
```
npm install text-encoding buffer   #for gun/sea deps
npm install github:bmatusiak/gun-rebuild github:bmatusiak/gun-rebuild-sea github:amark/gun   #for gun/sea
npm install react-native-webview react-native-webview-crypto expo-crypto  #for crypto
npm install @react-native-async-storage/async-storage   #for gun storage
```

# step3: loading gun (App.js)
```js
import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View } from 'react-native';
import React, { useRef, useEffect } from 'react';
import PolyfillCrypto from 'react-native-webview-crypto';
import * as Crypto from 'expo-crypto';
window.crypto.getRandomBytes = Crypto.getRandomBytes;
window.crypto.getRandomBytesAsync = Crypto.getRandomBytesAsync;
window.crypto.getRandomValues = Crypto.getRandomValues;
import 'gun/lib/mobile';
import Gun from 'gun';
import 'gun/sea';
import 'gun/lib/radix.js';
import 'gun/lib/radisk.js';
import 'gun/lib/store.js';
import AsyncStorage from '@react-native-async-storage/async-storage';
import asyncStore from 'gun/lib/ras.js';

export default function App() {
  var gun = (gun_ref => {
    if (gun_ref.current == null) {
      gun_ref.current = Gun({
        peers: 'https://gun:8765/gun',
        store: asyncStore({ AsyncStorage }),
      });
    }
    return gun_ref.current;
  })(useRef(null));
  useEffect(function () {
    (async function () {
      var pair = await Gun.SEA.pair();
      gun.user().auth(pair, function () {
        console.log(gun.user().is);
      });
    })();
  }, []);
  return (<>
    <PolyfillCrypto />
    <View style={styles.container}>
      <Text>Open up App.js to start working on your app!</Text>
      <StatusBar style="auto" />
    </View>
  </>);
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
});


```

```
npm run android
```