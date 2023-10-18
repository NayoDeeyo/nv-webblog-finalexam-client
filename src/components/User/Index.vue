<template>
  <div>
    <h1>Get All Car</h1>
    <p><button v-on:click="logout">Logout</button></p>
    <div>
      <div>
        <button v-on:click="navigateTo('/user/create')">สร้างข้อมูลยานพาหนะ</button>
      </div>
      <h2>จำนวนยานพาหนะ {{ users.length }}</h2>
    </div>
    <div v-for="i in users" v-bind:key="i.id">
      <div>brand: {{ i.name }}</div>
      <div>model: {{ i.lastname }}</div>
      <div>engine_capacity: {{ i.status }}</div>
      <div>category: {{ i.type }}</div>

      <div>
        <button v-on:click="navigateTo('/user/' + i.id)">ดูข้อมูลยานพาหนะ</button>
      </div>
      <div>
        <button @click="navigateTo('/user/edit/' + i.id)">edit car</button>
      </div>
      <div><button @click="deleteUser(i)">delete car</button></div>
      <hr />
    </div>
  </div>
</template>

<script>
import UsersService from "@/services/UsersService";

export default {
  data() {
    return {
      users: []
    };
  },
  methods: {
    logout() {
      this.$store.dispatch('setToken', null)
      this.$store.dispatch('setUser', null)
      this.$router.push({
        name: 'login'
      })
    },

    navigateTo(route) {
      this.$router.push(route);
    },
    async deleteUser(user) {
      let result = confirm("Want of delete?");
      if (result) {
        try {
          await UsersService.delete(user);
          this.refreshData();
        } catch (err) {
          console.log(err);
        }
      }
    },
    async refreshData() {
      this.users = (await UsersService.index()).data;
    }
  },
  async created() {
    try {
      this.users = (await UsersService.index()).data;
    } catch (err) {
      console.log(err);
    }
  }
};
</script>

<style></style>
