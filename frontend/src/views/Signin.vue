<template>
  <h1 style="margin-top: 100px ; margin-left: 280px;">Sign in</h1>
  <div class="h-100">
    <div class="container-fluid h-custom">
      <div class="row d-flex justify-content-center align-items-center h-100" style="margin-top: -12px;">
        <div class="col-md-9 col-lg-6 col-xl-5">
          <img src="https://www.centerklik.com/wp-content/uploads/2017/09/LoginPress-Login-Customizer.jpg"
            class="img-fluid" alt="Sample image">
        </div>
        <div class="col-md-8 col-lg-6 col-xl-4 offset-xl-1">
          <form  @submit="signin" >
            <div class="form-outline mb-4">
              <label class="form-label" for="username">Phone number</label>
              <input type="phone" class="form-control" v-model="phone" required />
            </div>

            <!-- Password input -->
            <div class="form-outline mb-3">
              <label class="form-label" for="pwd">Password</label>
              <input type="password" class="form-control" v-model="password" required />
            </div>
            <div class="d-flex justify-content-between align-items-center">
              <!-- Checkbox -->

            </div>
            <div class="text-center tex t-lg-start mt-4 pt-2 " >
              <button type="submit" class="btn btn-primary mt-2 py-0" style="
                  position: relative;
                  top: -21px;
                  margin-right: 121px;
                  height: 47px;
                  width: 127px;

">
                Login
                <div v-if="loading" class="spinner-border spinner-border-sm" role="status">
                  <span class="sr-only">Loading...</span>
                </div>
              </button>


            </div>
          </form>
          <p class="small fw-bold mt-2 pt-1 mb-0">Don't have an account yet? <router-link :to="{ name: 'Signup' }"
              class="small fw-bold mt-2 pt-1 mb-0" style="color: red;">
              Register
            </router-link>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>



<script>
import UserService from "@/services/user.service";
export default {
  name: "Signin",
  props: ["baseURL"],
  data() {
    return {
      email: null,
      password: null,
      loading: null,
      token: null,
    };
  },
  methods: {
    async signin(e) {
      e.preventDefault();
      this.loading = true;

      const user = {
        phone: this.phone,
        password: this.password,
      };
      const login = await UserService.login(user);
      console.log(login);
      localStorage.setItem("token", login.refresh_token);
      this.$emit("fetchData");
      if (login.message == "SUCCESS") {
        this.token = localStorage.getItem("token");
        this.$router.push({ name: "Home" });
      }
    },
  },
  mounted() {
    this.loading = false;
  },
};
</script>

<style scoped>

h1, h2, h3, h4, h5, h6 {
    color: #2B2D42;
    font-weight: 700;
    margin: 0 0 25px;
}

.text-center {
  margin-top: 64px;
  margin-left: 138px;
}

h1, h2, h3, h4, h5 h6 {
    color: #2B2D42;
    font-weight: 700;
    margin: 0 0px -20px;
    margin-top: 100px;
    margin-left: 100px;
}

.h1, h1 {
    font-size: 3.5rem;
}

</style>

