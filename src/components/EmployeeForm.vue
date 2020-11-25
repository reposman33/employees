<template lang="html">
  <div id="employee-form">
    <form v-on:submit.prevent="handleSubmit">
      <label for="">Company name</label>
      <input
        type="text"
        v-model="employee.company.name"
        ref="companyName"
        :class="{'has-error': submitting && invalidName}"
        @focus="clearStatus"
        @keypress="clearStatus">
      <label for="">Employee name</label>
      <input
        type="text"
        v-model="employee.name"
        :class="{'has-error': submitting && invalidName}"
        @focus="clearStatus"
        @keypress="clearStatus">
      <label for="">Employee email</label>
      <input
        type="text"
        v-model="employee.email"
        :class="{'has-error': submitting && invalidEmail}"
        @focus="clearStatus"
        @keypress="clearStatus">
        <p v-if="error && submitting" class="error-message">Please enter a username and email address</p>
        <p v-if="success">Employee added succesfully</p>
      <button>Add employee</button>
    </form>
  </div>
</template>

<script lang="js">

  export default  {
    name: 'employee-form',
    props: [],
    mounted () {

    },
  computed: {
    invalidName(){
      return this.employee.name === '';
    },
    invalidEmail(){
      return this.employee.email === '';
    },
    invalidCompanyName() {
      return this.employee.company.name === '';
    },
  },
    data () {
      return {
        submitting: false,
        success: false,
        error: false,
        employee: {
          company: {name:''},
          name: '',
          email:''
        }
      }
    },
    methods: {
      clearEmployee(){
        this.employee = {
          company: {name:''},
          name:'',
          email:''
          }
      },
      handleSubmit(){
        this.submitting = true;
        if(this.invalidName || this.invalidEmail || this.invalidCompanyName) {
          this.error = true;
          return ;
        }
        this.$emit('add:employee', this.employee)
        this.error = false;
        this.success = true;
        this.submitting = false;
        this.clearEmployee();
        this.$refs.companyName.focus()
      },
      clearStatus(){
        this.submitting = false;
        this.success = false;
        this.error = false;
      }
    },
}
</script>

<style scoped lang="scss">
  form {
    margin-bottom: 2rem;

    [class*='-message'] {
      font-weight: 500;
    }

    .error-message {
      color: red;
    }

    .success-message {
      color: green;
    }

  }
</style>
