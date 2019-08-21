<template>
  <section class="container">

    <h1>View Contact</h1>

    <div class="contact--section">
      <p class="__name">{{firstname}} {{lastname}}</p>

    </div>
  </section>
</template>

<script>
import db from './firebaseInit'
export default {
  name: 'view-contact',
  data () {
    return {
      firstname: null,
      lastname: null
    }
  },
  beforeRouteEnter (to, from, next) {
    db.collection('contacts').where('slug', '==', to.params.person).get().then((querySnapshot) => {
      querySnapshot.forEach((doc) => {
        next(vm => {
          vm.firstname = doc.data().firstname
          vm.lastname = doc.data().lastname
        })
      })
    })
  },
  watch: {
    '$route': 'fetchData'
  },
  methods: {
    fetchData () {
      db.collection('contacts').where('slug', '==', this.$route.params.person).get().then((querySnapshot) => {
        querySnapshot.forEach((doc) => {
          console.log(doc.id, ' => ', doc.data())
          this.firstname = doc.data().firstname
          this.lastname = doc.data().lastname
        })
      })
    }
  }
}
</script>
