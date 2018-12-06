<template>
  <div id='dashboard'>
    <ul class="collection with-header">
      <li class="collection-header"><h4>Employees</h4></li>
      <li v-for="employee in employees" v-bind:key="employee.id" class="collection-item">
        <div class="chip">{{employee.dept}}</div>{{employee.employee_id}}:{{employee.name}}
        <router-link class="secondary-content" v-bind:to="{name: 'view-employee', params: {employee_id: employee.employee_id}}"><i class="fa fa-eye"></i></router-link>
      </li>
    </ul>
    <div class="fixed-action-btn">
      <router-link to='/new' class="btn-floating btn-large red">
      <i class="fa fa-plus"></i></router-link>
    </div>
  </div>
</template>

<script>
import db from "./firebaseInit.js"
export default {
  name: "dashboard",
  data() {
    return {
      employees: []
    }
  },
  created() {
    db
      .collection("employees")
      .orderBy("dept")
      .get()
      .then(querySnapshot => {
        // console.log(querySnapshot.docs)
        querySnapshot.docs.forEach(element => {
          // console.log(element.data())
          const data = {
            id: element.id,
            employee_id: element.data().employee_id,
            name: element.data().name,
            dept: element.data().dept,
            position: element.data().position
          }
          this.employees.push(data)
        })
      })
    console.log(this.employees)
  }
}
</script>
