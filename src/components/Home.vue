<template>
  <section>
    <div>
      <div>
        <h1>All Contacts</h1>

        <router-link to="/add">Add New Contact</router-link>

        <div v-for="person in contacts"  v-bind:key="person.id">
          <div>
            <div>
              <p>{{person.firstname}} {{person.lastname}}</p>
            </div>
            <div>
              <router-link v-bind:to="{ name: 'view-contact', params: { person: person.slug }}">View Persossn</router-link>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>

</template>

<script>
import db from './firebaseInit'
export default {
  name: 'home',
  data () {
    return {
      contacts: []
    }
  },
  created () {
    db.collection('contacts').get().then((querySnapshot) => {
      querySnapshot.forEach((doc) => {
        let data = {
          'id': doc.id,
          'firstname': doc.data().firstname,
          'lastname': doc.data().lastname,
          'slug': doc.data().slug
        }
        this.contacts.push(data)
      })
    })
  }
}
</script>
