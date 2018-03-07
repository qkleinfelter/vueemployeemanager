<template>
  <div id="view-employee">
      <ul class="collection with-header">
        <li class="collection-header"><h4>{{name}}</h4></li>
        <li class="collection-item">Employee ID: {{euid}}</li>
        <li class="collection-item">Department: {{dept}}</li>
        <li class="collection-item">Position: {{position}}</li>
      </ul>
      <router-link to="/" class="btn grey">Back</router-link>
      <button @click="deleteEmployee" class="btn red">Delete</button>
  </div>
</template>

<script>
import db from './firebaseInit'
export default {
  name: 'view-employee',
  data() {
      return {
          euid: null,
          name: null,
          dept: null,
          position: null
      }
  },
  beforeRouteEnter(to, from, next) {
    db.collection('employees').where('euid', '==', to.params.euid).get().then(querySnapshot => {
      querySnapshot.forEach(doc => {
        next(vm => {
          vm.euid = doc.data().euid
          vm.name = doc.data().name
          vm.dept = doc.data().dept
          vm.position = doc.data().position
        })
      })
    })
  },
  watch: {
    '$route': 'fetchData'
  },
  methods: {
    fetchData() {
      db.collection('employees').where('euid', '==', this.$route.params.euid).get()
      .then(querySnapshot => {
        querySnapshot.forEach(doc => {
          this.euid = doc.data().euid
          this.name = doc.data().name
          this.dept = doc.data().dept
          this.position = doc.data().position
        })
      })
    },
    deleteEmployee(){
      if(confirm('Are you sure you want to delete this employee? This change CANNOT be undone')){
        db.collection('employees').where('euid', '==', this.$route.params.euid).get()
        .then(querySnapshot => {
        querySnapshot.forEach(doc => {
          doc.ref.delete();
          this.$router.push('/')
        })
      })
      }
    }
  }
}
</script>

