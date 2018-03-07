<template>
  <div id="edit-employee">
      <h3>Edit Employee</h3>
      <div class="row">
        <form @submit.prevent="updateEmployee" class="col s12">
          <div class="row">
            <div class="input-field col s12">
              <input type="text" v-model="euid" required disabled> 
            </div>
          </div>
          <div class="row">
            <div class="input-field col s12">
              <input type="text" v-model="name" required>
            </div>
          </div>
          <div class="row">
            <div class="input-field col s12">
              <input type="text" v-model="dept" required>
            </div>
          </div>
          <div class="row">
            <div class="input-field col s12">
              <input type="text" v-model="position" required>
            </div>
          </div>
          <button type="submit" class="btn">Submit</button>
          <router-link to="/" class="btn grey">Cancel</router-link>
        </form>
      </div>
  </div>
</template>

<script>
import db from './firebaseInit'
export default {
  name: 'edit-employee',
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
    updateEmployee() {
      db.collection('employees').where('euid', '==', this.$route.params.euid).get()
      .then(querySnapshot => {
        querySnapshot.forEach(doc => {
          doc.ref.update({
            euid: this.euid,
            name: this.name,
            dept: this.dept,
            position: this.position
          })
          .then(() => {
            this.$router.push({name: 'view-employee', params: {euid: this.euid}})
          })
        })
      })
    }
  }
}
</script>

