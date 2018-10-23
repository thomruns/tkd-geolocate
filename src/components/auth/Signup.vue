<template>
  <div class="signup container">
    <form @submit.prevent="signup" class="card-panel">
      <h2 class="center deep-purple-text">Sign up</h2>
      <div class="field">
        <label for="email">Email:</label>
        <input type="email" name="email" v-model="email">
      </div>
      <div class="field">
        <label for="password">Password (6 char minimum):</label>
        <input type="password" name="password" v-model="password">
      </div>
      <div class="field">
        <label for="alias">Screenname:</label>
        <input type="text" name="alias" v-model="alias">
        <p class="red-text center" v-if="feedback">{{ feedback }}</p>
      </div>
      <div class="field center">
        <button class="btn deep-purple">Sign up</button>
      </div>
    </form>
  </div>
</template>

<script>
import slugify from 'slugify' // to create a slug from input
import db from '@/firebase/init' // get the Firebase database
import firebase from 'firebase' // get the Firebase library
import functions from 'firebase/functions' // uset Firebase functions

export default {
  name: 'Signup',
  data() {
    return {
      email: null,
      password: null,
      alias: null,
      feedback: null,
      slug: null
    }
  },
  methods: {
    signup() {
      if(this.alias && this.email && this.password) {
        this.slug = slugify(this.alias, {
          replacement: '-',
          remove: /[$*_+~.()'"!\-:@]/g,
          lower: true
        })
        let checkAlias = firebase.functions().httpsCallable('checkAlias')
        checkAlias({ slug: this.slug }).then(result => {
          console.log(result)
          if(!result.data.unique) {
            this.feedback = "This username already exists"
          } else {
            firebase.auth().createUserWithEmailAndPassword(this.email, this.password)
            .then(cred => {
              db.collection('users').doc(this.slug).set({
                alias: this.alias,
                geolocation: null,
                user_id: cred.user.uid
              })
              //console.log(cred.user)
            })
            .then(() => {
              this.$router.push({ name: 'GMap' })
            })
            .catch(err => {
              console.log(err)
              this.feedback = err.message
            })
            this.feedback = "You may use this username"
          }
        })
      } else {
        this.feedback = "You must enter all fields"
      }
    }
  }
}
</script>

<style>
.signup {
  max-width: 400px;
  margin-top: 60px;
}

.signup h2 {
  font-size: 2.4em;
}

.signup .field {
  margin-bottom: 16px;
}

</style>


