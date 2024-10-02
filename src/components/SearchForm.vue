<template>
   <div>
      <form @submit.prevent="submitForm">
         <div>
            <label for="email">Email:</label>
            <input type="email" v-model="email" required />
         </div>

         <div>
            <label for="number">Number:</label>
            <input type="text" v-model="number" @input="formatNumber" />
         </div>

         <button type="submit">Submit</button>
      </form>

      <div v-if="loading">Loading...</div>
      <div v-if="users">
         <h3>Found Users:</h3>
         <ul>
            <li v-for="user in users" :key="user.email">
               {{ user.email }} - {{ user.number || "No number" }}
            </li>
         </ul>
      </div>
   </div>
</template>

<script>
import axios from "axios";

export default {
   data() {
      return {
         email: "",
         number: "",
         users: [],
         loading: false,
         cancelTokenSource: null,
      };
   },
   methods: {
      formatNumber() {
         this.number = this.number
            .replace(/[^0-9]/g, "")
            .replace(/(\d{2})(?=\d)/g, "$1-")
            .slice(0, 8);
      },
      async submitForm() {
         if (this.cancelTokenSource) {
            this.cancelTokenSource.cancel("Request canceled by user");
         }

         this.cancelTokenSource = axios.CancelToken.source();

         this.loading = true;
         try {
            const response = await axios.post(
               "http://localhost:3000/api/search",
               {
                  email: this.email,
                  number: this.number.replace(/-/g, ""),
               },
               { cancelToken: this.cancelTokenSource.token }
            );

            this.users = response.data;
         } catch (error) {
            if (axios.isCancel(error)) {
               console.log("Request canceled:", error.message);
            } else {
               console.error("Error:", error);
            }
         } finally {
            this.loading = false;
         }
      },
   },
};
</script>

<style scoped>
form {
   margin-bottom: 20px;
}

input {
   margin-bottom: 10px;
}

button {
   margin-top: 10px;
}
</style>
