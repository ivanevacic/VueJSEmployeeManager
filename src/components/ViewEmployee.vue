<template>
  <div id="view-employee">
    <ul class="collection with-header">
      <li class="collection-header"><h4>{{name}}</h4></li>
      <li class="collection-item">Employee ID#:{{employee_id}}</li>
      <li class="collection-item">Department:{{department}}</li>
      <li class="collection-item">Position:{{position}}</li>
    </ul>
    <router-link to="/" class="btn grey">Back</router-link>
    <button @click="deleteEmployee" class="btn red">Delete</button>
  </div>
</template>

<script>
import db from './firebaseInit'

export default {
  name: 'view-employee',
  data () {
    return {
      employee_id: null,
      name: null,
      department: null,
      position: null
    }
  },
  //  Fetch individual employee(id)
  beforeRouteEnter(to, from, next) {
    // Get employee with searched id
    db.collection('employees').where('employee_id','==', to.params.employee_id).get()
      .then(querySnapshot => {
        querySnapshot.forEach(doc => {
          next(vm => {
            //  Populate empty object with values from DB for searched employee
            vm.employee_id = doc.data().employee_id,
            vm.name = doc.data().name,
            vm.department = doc.data().department,
            vm.position = doc.data().position
          })
        })
      })
  },
  // Add watcher
  watch: {
    '$route': 'fetchData'
  },
  methods: {
    fetchData () {
      db.collection('employees').where('employee_id', '==', this.$route.params.employee_id).get()
        .then(querySnapshot => {
          querySnapshot.forEach(doc => {
            this.employee_id = doc.data().employee_id
            this.name = doc.data().name
            this.department = doc.data().department
            this.position = doc.data().position
          })
        })
    },
    deleteEmployee () {
    if(confirm('Are your sure?')) {
      db.collection('employees').where('employee_id', '==', this.$route.params.employee_id).get()
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

