<template>
  <div id="new-employee">
      <h3>New Employee</h3>
      <div class="row">
        <form @submit.prevent="saveEmployee" class="col s12">
          <div class="row">
            <div class="input-field col s12">
              <input type="text" v-model="euid" required>
              <label>Employee ID</label>
            </div>
          </div>
          <div class="row">
            <div class="input-field col s12">
              <input type="text" v-model="name" required>
              <label>Name</label>
            </div>
          </div>
          <div class="row">
            <div class="input-field col s12">
              <input type="text" v-model="dept" required>
              <label>Department</label>
            </div>
          </div>
          <div class="row">
            <div class="input-field col s12">
              <input type="text" v-model="position" required>
              <label>Position</label>
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
  name: 'new-employee',
  data() {
      return {
          euid: null,
          name: null,
          dept: null,
          position: null
      }
  },
  methods: {
    saveEmployee(){
      db.collection('employees').add({
        euid: this.euid,
        name: this.name,
        dept: this.dept,
        position: this.position
      })
      .then(docRef => {
        this.$router.push({name: 'view-employee', params: {euid: this.euid}})
      })
      .catch(err => console.log(err))
    }
  }
}
</script>

