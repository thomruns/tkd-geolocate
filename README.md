# tkd-geolocate

> A Vue.js project using Google Maps API, webpack, and Firebase/Firestore

IMPORTANT NOTE: A directory should be created in the src directory, called firebase, into which an init.js file should be created. In this file should be placed the config code that is copied from the Firebase setup process, as well as this code:

``` bash
import firebase from 'firebase'
import firestore from 'firebase/firestore'

 ...FIREBASE CONFIG CODE...

const firebaseApp = firebase.initializeApp(config);
firebaseApp.firestore().settings({ timestampsInSnapshots: true })

export default firebaseApp.firestore()
```

Also note that the firebase.initializeApp() function is stored in a constant for export.

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
