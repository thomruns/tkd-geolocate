<template>
  <div class="navbar">
    <nav class="deep-purple darken-1">
      <div class="container">
        <a href="" class="brand-logo left">GeoLocate Users</a>
        <ul class="right">
          <li><router-link :to="{ name: 'Signup' }">Sign up</router-link></li>
          <li><router-link :to="{ name: 'Login' }">Log in</router-link></li>
          <li><a @click="logout">Log out</a></li>
        </ul>
      </div>
    </nav>
  </div>
</template>

<script>
import firebase from 'firebase'

export default {
  name: 'Navbar',
  data() {
    return {

    }
  },
  methods: {
    logout() {
      firebase.auth().signOut().then(() => {
        this.$router.push( { name: 'Signup' })
      })
    }
  }
}
</script>

<style>

</style>


