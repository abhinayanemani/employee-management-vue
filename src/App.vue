<template>
  <div class="container mt-4">

    <!-- Header -->
    <h2 class="text-center text-primary mb-4">
      Employee Management System
    </h2>

    <!-- Form -->
    <div class="card p-4 shadow-sm mb-4">

      <input v-model="name" placeholder="Name" class="form-control mb-2" />
      <input v-model="designation" placeholder="Designation" class="form-control mb-2" />
      <input v-model="department" placeholder="Department" class="form-control mb-2" />
      <input v-model="salary" placeholder="Salary" class="form-control mb-3" />

      <!-- Buttons -->
      <div>
        <button 
          v-if="!editId" 
          @click="addEmployee" 
          class="btn btn-primary">
          ➕ Add Employee
        </button>

        <button 
          v-else 
          @click="updateEmployee" 
          class="btn btn-success">
          ✔ Update
        </button>

        <button 
          v-if="editId" 
          @click="cancelEdit" 
          class="btn btn-secondary ms-2">
          ❌ Cancel
        </button>
      </div>
    </div>

    <!-- Table -->
    <table class="table table-bordered table-striped table-hover shadow-sm">
      <thead class="table-dark">
        <tr>
          <th>Name</th>
          <th>Designation</th>
          <th>Department</th>
          <th>Salary</th>
          <th>Actions</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="emp in employees" :key="emp.id">
          <td>{{ emp.name }}</td>
          <td>{{ emp.designation }}</td>
          <td>{{ emp.department }}</td>
          <td>{{ emp.salary }}</td>
          <td>
            <button 
              @click="editEmployee(emp)" 
              class="btn btn-warning btn-sm me-2">
              ✏️
            </button>

            <button 
              @click="deleteEmployee(emp.id)" 
              class="btn btn-danger btn-sm">
              🗑
            </button>
          </td>
        </tr>
      </tbody>
    </table>

  </div>
</template>

<script>
import axios from "axios";

const API_URL = "https://69fa2715c509a40d3aa3fb76.mockapi.io/api/employees";

export default {
  data() {
    return {
      employees: [],
      name: "",
      designation: "",
      department: "",
      salary: "",
      editId: null
    };
  },

  mounted() {
    this.getEmployees();
  },

  methods: {
    async getEmployees() {
      const res = await axios.get(API_URL);
      this.employees = res.data;
    },

    async addEmployee() {
      if (!this.name || !this.designation) {
        alert("Please fill all fields");
        return;
      }

      await axios.post(API_URL, {
        name: this.name,
        designation: this.designation,
        department: this.department,
        salary: this.salary
      });

      this.resetForm();
      this.getEmployees();
    },

    editEmployee(emp) {
      this.name = emp.name;
      this.designation = emp.designation;
      this.department = emp.department;
      this.salary = emp.salary;
      this.editId = emp.id;
    },

    async updateEmployee() {
      await axios.put(`${API_URL}/${this.editId}`, {
        name: this.name,
        designation: this.designation,
        department: this.department,
        salary: this.salary
      });

      this.resetForm();
      this.getEmployees();
    },

    async deleteEmployee(id) {
      await axios.delete(`${API_URL}/${id}`);
      this.getEmployees();
    },

    cancelEdit() {
      this.resetForm();
    },

    resetForm() {
      this.name = "";
      this.designation = "";
      this.department = "";
      this.salary = "";
      this.editId = null;
    }
  }
};
</script>

<style>
body {
  background-color: #f8f9fa;
}
</style>