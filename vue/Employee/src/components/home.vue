<template>
  <div>
   
    <div>
      <h3>Add Department</h3>
      Department name:<input type="text" v-model="DepartmentName">
      <button v-on:click="createClick">Add</button>
    </div>
    <div>
      <h3>Departments List</h3>
      <ul>
        <li v-for="dept in departments" :key="dept.DepartmentId">
          {{ dept.DepartmentId }} - {{ dept.DepartmentName }}
          <input type="text" v-model="dept.newName" placeholder="Update Name">
          <button v-on:click="updateDepartment(dept)">Update</button>
          <button v-on:click="deleteDepartment(dept.DepartmentId)">Delete</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: "home",
  data() {
    return {
      departments: [],
      DepartmentName: ""
    };
  },
  methods: {
    async createClick() {
      try {
        let response = await axios.post("https://localhost:44319/api/Department", {
          DepartmentName: this.DepartmentName
        });
        alert(response.data);
        this.DepartmentName = "";  // Clear the input field after adding
        this.getDepartments();  // Fetch the updated list of departments
      } catch (error) {
        console.error(error);
      }
    },
    async updateDepartment(dept) {
       
        try {
          let response = await axios.put("https://localhost:44319/api/Department", {
            DepartmentId:dept.DepartmentId,
            DepartmentName:dept.newName
          });
          alert(response.data);
            // Clear the input field after updating
          this.getDepartments();  // Fetch the updated list of departments
        } catch (error) {
          console.error(error);
        }
      
    },
    async deleteDepartment(id) {
      try {
        let response = await axios.delete(`https://localhost:44319/api/Department/${id}`);
        alert(response.data);
        this.getDepartments();  // Fetch the updated list of departments
      } catch (error) {
        console.error(error);
      }
    },
    async getDepartments() {
      try {
        let result = await axios.get("https://localhost:44319/api/Department");
        // Add newName property to each department for update input
        this.departments = result.data.map(dept => ({ ...dept, newName: "" }));
      } catch (error) {
        console.error(error);
      }
    }
  },
  async mounted() {
    await this.getDepartments();  // Fetch the list of departments when the component is mounted
  }
};
</script>
