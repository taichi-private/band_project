<template>
  <section>
    <h1>Add New Contact</h1>

    <router-link to="/">topへ戻る</router-link>

    <form @submit.prevent="saveContact">
      <div>
        <label>First Name</label>
        <div>
          <input type="text" placeholder="First Name" v-model="firstname" required>
        </div>
      </div>
      <div>
        <label>Last Name</label>
        <div>
          <input type="text" placeholder="Last Name" v-model="lastname" required>
        </div>
      </div>

      <div>
        <div>
          <button type="submit" v-bind:disabled="val">Submit</button>
        </div>
      </div>
    </form>
    <button v-on:click="resetContents">リセット</button>
    <span v-if="val">登録されました</span>
  </section>
</template>
<script>
import db from './firebaseInit'
export default {
  name: 'new-contact',
  data () {
    return {
      firstname: null,
      lastname: null,
      val: false
    }
  },
  methods: {
    saveContact () {
      db.collection('contacts').add({
        firstname: this.firstname,
        lastname: this.lastname,
        slug: this.generateUUID()
      }).then(function (docRef) {
      }).catch(function (error) {
        console.error('Error adding document: ', error)
      })
      this.val = !this.val
    },
    generateUUID () {
      let d = new Date().getTime()
      let uuid = 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function (c) {
        let r = (d + Math.random() * 16) % 16 | 0
        d = Math.floor(d / 16)
        return (c === 'x' ? r : (r & 0x3 | 0x8)).toString(16)
      })
      return uuid
    },
    resetContents () {
      this.firstname = null
      this.lastname = null
    }
  }
}
</script>
