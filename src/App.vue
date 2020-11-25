<template>
  <div id="app" class="small-container">
    <h1>Employees</h1>

    <employee-form v-on:add:employee="addEmployee" />
    <employee-table
      v-bind:employees="employees"
      v-on:delete:employee="deleteEmployee"
      @update:employee="updateEmployee"
    />

  </div>
</template>

<script>
import EmployeeTable from '@/components/EmployeeTable.vue'
import EmployeeForm from '@/components/EmployeeForm.vue'
import {TYPICODE_USERS_URL} from "./components/constants";

export default {
  name: 'App',
  components: {
    EmployeeTable,
    EmployeeForm
  },
  data: () => ({
    employees: []
  }),
  mounted () {
      this.getEmployees();
    },
  methods: {
    async getEmployees() {
      const response = await fetch(TYPICODE_USERS_URL);
      const data = await response.json();
      this.employees = data;
     },
     async addEmployee(employee) {
       try {
         const response = await fetch(TYPICODE_USERS_URL, {
           method: 'POST',
           body: JSON.stringify(employee),
           headers: {'Content-type':'application/json', 'charset':'utf-8'}
        });
        const data = await response.json();
        this.employees = [...this.employees, data];
     } catch(e) {
       console.error(e);
     }
    },
    async deleteEmployee(id) {
      try {
        await fetch(TYPICODE_USERS_URL+"/"+id,{
          method:"DELETE"
        });
        this.employees = this.employees.filter(employee => employee.id !== id)
      } catch(e) {
        console.error(e)
      }
    },
    async updateEmployee(updatedEmployee) {
      try {
        console.log("updating employee...", updatedEmployee);
        await fetch(TYPICODE_USERS_URL+"/"+updatedEmployee.id,{
          method: "PUT",
          headers: {'Content-type': 'application/json', 'charset': 'utf-8'},
          body: JSON.stringify(updatedEmployee)
        })
      } catch(e) {
        console.error(e);
      }
              console.log("employee updated");
      this.employees = this.employees
      .map(employee => employee = employee.id === updatedEmployee.id ? updatedEmployee : employee);
    }
  }
}
</script>

<style>
button {
    background: #009435;
    border: 1px solid #009435; 
}

.small-container {
max-width: 1280px;
}
</style>
