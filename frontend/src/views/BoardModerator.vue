<!-- /*
@Author: Saijivan Maheswaran
@version: 1.0v
@Latest Update: 24.06.2021
Status: Done

Descripton:  Student Record Management system. It efficiently allows users to perform CRUD operations.
             Schüler Datenbank Mangament system. Erlaubt den Nutzer effizient CRUD Operationen durchzuführen.

*/
-->


<template >
  <div class="overflow-auto">
    <div class="row">
      <div class="col-md-12">
        <br>
        <br>
         <router-link to="/create-teacher" class="btn btn-success" v-if="currentUser.roles.includes('ROLE_ADMIN')">Add Teacher</router-link>
        <br>
        <br>
        <table class="table table-striped">
          <thead class="thead-dark">
            <tr>
              <th>First name</th>
              <th>Last name</th>
              <th>Class</th>
                <th  v-if="currentUser.roles.includes('ROLE_ADMIN')" >Actions</th>
            </tr>
          </thead>
         <tbody>
            <tr v-for="teacher in Teachers" :key="teacher._id">
              <td>{{ teacher.firstName }}</td>
              <td>{{ teacher.lastName }}</td>
              <td>{{ teacher.class }}</td>
               <td v-if="currentUser.roles.includes('ROLE_ADMIN')">
                <router-link
                  :to="{ name: 'edit-teacher', params: { id: teacher._id } }"
                  class="btn btn-warning"
                  >Edit
                </router-link>
                <button
                  @click.prevent="deleteTeacher(teacher._id)"
                  class="btn btn-danger"
                >
                  Delete
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <!--<div>
      Sorting By: <b>{{ sortBy }}</b
      >, Sort Direction:
      <b>{{ sortDesc ? "Descending" : "Ascending" }}</b>
    </div> 
    <br />-->
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      sortBy: "firstname",
      sortDesc: false,
      perPage: 3,
      currentPage: 1,
      Teachers: [],
  name: 'Profile',
  computed: {
    currentUser() {
      return this.$store.state.auth.user;
    }
  },
  mounted() {
    if (!this.currentUser) {
      this.$router.push('/login');
    }
  }
};
  },

  created() {
    let apiURL = "http://localhost:8080/api/lehrer";
    axios
      .get(apiURL)
      .then((res) => {
        this.Teachers = res.data;
      })
      .catch((error) => {
        console.log(error);
      });
  },
  methods: {
    deleteTeacher(id) {
      let apiURL = `http://localhost:8080/api/delete-Teachers/${id}`;
      let indexOfArrayItem = this.Teachers.findIndex((i) => i._id === id);

      if (window.confirm("Do you really want to delete?")) {
        axios
          .delete(apiURL)
          .then(() => {
            this.Teachers.splice(indexOfArrayItem, 1);
          })
          .catch((error) => {
            console.log(error);
          });
      }
    },
  },
  computed: {
    rows() {
      return this.Teachers.length;
    },
    currentUser() {
      return this.$store.state.auth.user;
  }},
};

</script>

<style>
.btn-success {
  margin-right: 10px;
}
</style>