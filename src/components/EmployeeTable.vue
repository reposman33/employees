<template lang="html">
  <div id="employee-table">
    <p v-if="employees.length < 1" class='empty-table'> No employees</p>
    <table v-else>
      <thead>
        <tr>
          <th>Company name</th>
          <th>Employee name</th>
          <th>Employee email</th>
        </tr>
      </thead>
      <tbody>
        <tr :key="employee.id" v-for="employee in employees">
          <td :style="stylesEditingTd" v-if="editing === employee.id">
            <input type="text" v-model="employee.company.name">
          </td>
          <td :title="employee.company.bs" v-else>{{employee.company.name}}</td>

          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.name">
          </td>
          <td v-else>{{employee.name}}</td>
          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.email">
          </td>
          <td v-else>{{employee.email}}</td>
          <td v-if="editing === employee.id">
            <button @click="updateEmployee(employee)">Save</button>
            <button class="muted-button" @click="cancelEdit(employee)">Cancel</button>
          </td>
          <td v-else>
            <button @click="editMode(employee)">Edit</button>
            <button @click="$emit('delete:employee',employee.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script lang="js">
  export default  {
    name: 'employee-table',
    props: {
      employees: Array
    },
    data () {
      return {
        editing: null,
        employee: {
          company: {name:''},
          name:'',
          email:''
        },
        cachedEmployee: null
      }
    },
    methods: {
      updateEmployee(employee) {
        if(employee.name === '' || employee.email === '' || employee.company.name === '') {
          return;
        }
        this.$emit('update:employee', employee);
        this.editing = null;
     },
     editMode(employee){
       this.editing = employee.id;
       this.cachedEmployee = Object.assign({}, employee)
     },
     cancelEdit(employee){
       Object.assign(employee, this.cachedEmployee)
       this.editing = null;
     }
  }
}
</script>

<style scoped lang="scss">
  button {
    margin: 0 0.5rem 0 0;
    :visited {
      color: #009435;
    }

  }
</style>
