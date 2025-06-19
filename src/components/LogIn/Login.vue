<template>
  <section class="login-wrapper">
    <div class="container">
      <div class="row justify-content-center text-center text-white mt-4 mb-3">
        <div>
          <a href="https://ezwork.vn/" class="text-white" target="_blank"><strong>ezWork</strong></a>
          <span style="margin: 4px;"><strong> | </strong></span>
          <a href="https://ezwater.vn/" class="text-white" target="_blank"><strong>ezWater</strong></a>
        </div>
      </div>

      <div class="row justify-content-center align-items-center" style="min-height: 75vh;">
        <div class="col-md-6 col-lg-4">
          <div class="box13 mb-3">
            <img src="../../assets/img/background.png" class="img-fluid img-thumbnail" alt="banner" />
            <div class="box-content">
              <h5 class="title noselect">Thêm một đơn vị mới ở TP.Hồ Chí Minh ứng dụng ezWork</h5>
            </div>
          </div>
          <img alt="logo" class="d-block mx-auto mb-4" style="width: 50%;" src="../../assets/img/logo_ezwork.png" />

          <form @submit.prevent="login" class="horizontal-form">
            <div class="form-group">
              <input
                type="text"
                v-model="username"
                placeholder="Tài khoản"
                class="form-control text-center font-weight-400"
                required
              />
            </div>
            <div class="form-group password">
              <input
                type="password"
                v-model="password"
                placeholder="Mật khẩu"
                class="form-control text-center font-weight-400"
                required
              />
            </div>
            <button
              class="btn yellow-crusta btn-scroll btn-scroll-top animated fadeIn mt-3 d-block mx-auto"
              type="submit"
              :disabled="!isValid || loading"
            >
              <i class="fa fa-spinner fa-pulse" v-if="loading"></i>
              <span class="text-uppercase font-weight-400"> Đăng nhập</span>
            </button>
          </form>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import axios from "axios";
import router from "@/router";
import { BASE_API_URL } from "@/main";

export default {
  data() {
    return {
      username: "",
      password: "",
      loading: false
    };
  },
  computed: {
    isValid() {
      return this.username && this.password;
    }
  },
  methods: {
    async login() {
      this.loading = true;
      try {
        const response = await axios.post(`${BASE_API_URL}/auth/login`, {
          email: this.username,
          password: this.password
        });
        const user = response.data.data;
        localStorage.setItem("email", user.email);
        localStorage.setItem("name", user.name);
        localStorage.setItem("role_id", user.role_id);
        localStorage.setItem("token", user.token);
        router.push("/");
      } catch (error) {
        alert("Sai tài khoản hoặc mật khẩu");
        console.error(error);
      } finally {
        this.loading = false;
      }
    }
  }
};
</script>

<style scoped>
.login-wrapper {
  background-image: linear-gradient(#115387 10%, #117BD1 70%, #11B6D4 100%);
  min-height: 100vh;
  padding: 20px 15px;
  color: #fff;
}
.password{
  margin-top: 10px;
}
.box14 {
  position: relative;
  overflow: hidden;
  transition: all 0.3s ease-in-out;
}
.box14 .box-content {
  position: absolute;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  width: 100%;
  padding: 12px;
  color: #fff;
  text-align: center;
}
</style>
