<template>
  <div>
    <h1>Edit Car</h1>
    <form v-on:submit.prevent="editUser">
      <p>brand: <input type="text" v-model="user.name" /></p>
      <p>model: <input type="text" v-model="user.lastname" /></p>
      <p>email: <input type="text" v-model="user.email" /></p>
      <p>password: <input type="text" v-model="user.password" /></p>
      <p><button type="submit">edit car</button></p>
    </form>
    <hr />
    <div>
      <p>brand: {{ user.name }}</p>
      <p>model: {{ user.lastname }}</p>
      <p>email: {{ user.email }}</p>
      <p>password: {{ user.password }}</p>
      <p></p>
    </div>
  </div>
</template>

<script>
import UsersService from "@/services/UsersService";

export default {
  data() {
    return {
      user: {
        name: "",
        lastname: "",
        email: "",
        password: "",
        status: "active"
      }
    };
  },
  methods: {
    async editUser() {
      try {
        console.log(this.user)
        await UsersService.put(this.user)
        this.$router.push("/users")
      } catch (err) {
        console.log(err)
      }
    }
  },
  async created() {
    try {
      let userId = this.$route.params.userId
      this.user = (await UsersService.show(userId)).data
    } catch (err) {
      console.log(err)
    }
  }
};
</script>

<style></style>
