# step1: react-native bare tempalte

```
npx react-native init AwesomeProject
```
or speific version (see versions set in 2 spots)
```
npx react-native@0.70.6 init AwesomeProject --version 0.70.6
```

# step2: install packages
```
npm install text-encoding buffer   #for gun/sea deps
npm install github:amark/gun github:bmatusiak/gun-unbuild github:bmatusiak/gun-rebuild-sea   #for gun/sea
npm install react-native-webview react-native-webview-crypto react-native-get-random-values  #for crypto
npm install @react-native-async-storage/async-storage   #for gun storage
```

# step3: loading gun (App.js)
```js
import PolyfillCrypto from 'react-native-webview-crypto'
import 'react-native-get-random-values';
import GUN from 'gun/src'
import SEA from 'gun/sea'
import 'gun/lib/radix.js'
import 'gun/lib/radisk.js'
import 'gun/lib/store.js'
import AsyncStorage from '@react-native-async-storage/async-storage';
import asyncStore from 'gun/lib/ras.js'
```

```js
var gun = Gun({
  peers: "https://gun:8765/gun",
  store: asyncStore({ AsyncStorage })
});
```
