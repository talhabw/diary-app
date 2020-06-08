<template>
  <div class="container" v-if="!$parent.login">
    <div class="login">
      <h1>LOGIN</h1>
      <span class="warning">{{warning}}</span>
      <div class="input">
        <i class="fas fa-user iconn"></i>
        <input type="text" v-model="username" placeholder="Username" />
      </div>
      <div class="input">
        <i class="fas fa-lock"></i>
        <input type="password" v-model="password" placeholder="Password" />
      </div>
      <button class="login-button" @click="loginControl">Login</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      username: "",
      password: "",
      warning: ""
    };
  },
  methods: {
    loginControl() {
      if (this.username == "deneme" && this.password == "123456") {
        this.warning = "";
        this.username = "";
        this.password = "";
        this.$parent.login = true;
      } else {
        this.warning = "Bilgilerinizi hatalı girdiniz!";
      }
    }
  }
};
</script>

<style scoped>
.container {
  width: 1200px;
  margin: 0 auto;
  display: flex;
  justify-content: center;
}
.login {
  display: flex;
  padding: 50px 50px;
  border-radius: 30px;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  text-align: center;
  color: #c3edea;
  background: #709fb0;
}

.input {
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-radius: 10px;
  background: white;
  width: 250px;
  font-size: 1.2rem;
  padding: 10px 10px;
  margin: 10px 0;
  outline: none;
  border: none;
  transition: border 300ms linear;
  color: rgba(0, 255, 191, 1);
  height: 32px;
}

.login input {
  border: none;
  height: 30px;
  width: 215px;
  transition: border 300ms linear;
}

.login input:focus {
  outline: none;
  border-bottom: 1px rgba(0, 255, 191, 1) solid;
}

.login-button {
  padding: 10px 50px;
  margin-top: 10px;
  border: none;
  border-radius: 7px;
  background: linear-gradient(
    90deg,
    rgba(0, 255, 191, 1) 40%,
    rgba(0, 183, 255, 1) 90%
  );
  outline: none;
  cursor: pointer;
}
</style>
