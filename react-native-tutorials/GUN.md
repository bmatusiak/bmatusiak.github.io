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
npm install github:bmatusiak/gun-unbuild github:bmatusiak/gun-rebuild-sea github:bmatusiak/gun#localStorage-fix   #for gun/sea
npm install react-native-webview react-native-webview-crypto react-native-get-random-values  #for crypto
npm install @react-native-async-storage/async-storage   #for gun storage
```

# step3: loading gun (App.js)
```js
import React, {useRef, useEffect} from 'react';
import PolyfillCrypto from 'react-native-webview-crypto';
import 'react-native-get-random-values';
import 'gun/lib/mobile';
import Gun from 'gun';
import 'gun/sea';
import 'gun/lib/radix.js';
import 'gun/lib/radisk.js';
import 'gun/lib/store.js';
import AsyncStorage from '@react-native-async-storage/async-storage';
import asyncStore from 'gun/lib/ras.js';

export default function App() {
  var gun = useRef(null);
  if (gun.current == null) {
    gun.current = Gun({
      peers: 'https://gun:8765/gun',
      store: asyncStore({AsyncStorage}),
    });
  }
  useEffect(function () {
    (async function () {
      var pair = await Gun.SEA.pair();
      gun.current.user().auth(pair, function () {
        console.log(gun.current.user().is);
      });
    })();
  }, []);
  return (
    <>
      <PolyfillCrypto />
    </>
  );
}

```
