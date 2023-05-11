<template>
    <div>
        <h2>Employee Registation</h2>
        <form @submit.prevent="save" >
            <div class="form-row">
                <div class="col-5">
                  <input type="text" v-model="employee.name" class="form-control" placeholder="Name">
                </div>
                <div class="col-5">
                  <input type="text" class="form-control" v-model="employee.address" placeholder="Address">
                </div>
                <div class="col-5">
                  <input type="text" class="form-control" v-model="employee.mobile" placeholder="Mobile">
                </div>
            </div>
            <button type="submit" class="btn btn-primary">Save</button>
        </form>
        <table class="table table-dark">
            <thead>
              <tr>
                <th scope="col">ID</th>
                <th scope="col">Employee Name</th>
                <th scope="col">Address</th>
                <th scope="col">Mobile</th>
                <th scope="col">Option</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="employee in result" v-bind:key="employee.id">
                <td>{{ employee.id }}</td>
                <td>{{ employee.name }}</td>
                <td>{{ employee.address }}</td>
                <td>{{ employee.mobile }}</td>
                <td>
                    <button type="button" class="btn btn-warning" @click="edit(employee)">Edit</button>
                    <button type="button" class="btn btn-danger" @click="remove(employee)">Delete</button>
                </td>
              </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import Vue from 'vue'
import axios from 'axios'
Vue.use(axios)
export default {
  name: 'EmployeeView',
  data () {
    return {
      result: {},
      employee: {
        id: '',
        name: '',
        address: '',
        mobile: ''
      }
    }
  },
  created () {
    this.Employeeload()
  },

  methods: {
    Employeeload () {
      console.log('LoadTask() calling...')
      var page = 'http://127.0.0.1:8000/api/employees'
      axios.get(page).then(({ data }) => {
        console.log(data)
        this.result = data
      })
    },
    save () {
      if (this.employee.id === '') {
        this.saveData()
      } else {
        this.updateEmployee()
      }
    },
    saveData () {
      axios.post('http://127.0.0.1:8000/api/save', this.employee)
        .then(
          ({data}) => {
            alert('saved')
            this.Employeeload()
          }
        )
    },

    edit (employee) {
      this.employee = employee
    },
    updateEmployee () {
      var editUrl = 'http://127.0.0.1:8000/api/update/' + this.employee.id
      axios.put(editUrl, this.employee)
        .then(
          ({data}) => {
            this.employee.name = ''
            this.employee.address = ''
            this.employee.mobile = ''
            this.employee.id = ''
            alert('UPDATED!!')
            this.Employeeload()
          }
        )
    },
    remove (employee) {
      var url = 'http://127.0.0.1:8000/api/delete/' + employee.id
      axios.delete(url)
      alert('Deleted!')
      this.Employeeload()
    }
  }
}
</script>
