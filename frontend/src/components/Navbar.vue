<template>
  <nav class="navbar gap-5">
    <router-link class="navbar-brand" :to="{ name: 'Home' }">
      <div>
        <h1 class="logo">Mobile Store</h1>
      </div>
    </router-link>
    <ul class="d-flex gap-5">
      <li>
        <router-link class="navbar-brand" :to="{ name: 'Home' }">
          Home
        </router-link>
      </li>
      <li>
        <router-link class="navbar-brand" :to="{ name: 'About' }">
          About Us
        </router-link>
      </li>
      <li>
        <router-link class="navbar-brand" :to="{ name: 'Contact' }">
          Contact
        </router-link>
      </li>
    </ul>
    <div class="">
      <ul class="nav-menu">
        <li v-if="!token" class="login-btn1" id="button">
          <a type="button">
            <router-link class="text-white" :to="{ name: 'Signin' }">
              <i class="fas fa-sign-in-alt"></i>
            </router-link>
          </a>
        </li>

        <li class="nav-item" v-if="token">
          <div class="login-btn">
            <i class="material-icons">person</i>
            <div class="hidden-content">
              <!-- Các thẻ con ẩn -->
              <p><a href="#">
                  <router-link class="" :to="{ name: 'User' }">
                    <a class="">Profile
                      <i class="fas fa-id-card"> </i> 
                    </a>
                  </router-link>
                </a></p>
              <p><a href="#">
                  <router-link class="" :to="{ name: 'Order' }">
                    <a class="">Order
                      <i class="material-icons">local_shipping</i>
                    </a>
                  </router-link>
                </a></p>
              <p><a v-if="token" href="#" @click="signout">Logout
                  <i class="fas fa-sign-out-alt"></i>
                </a></p>
            </div>
          </div>
        </li>


        <div class="navbar-cart" style="margin: 15px 100px 15px -150px;" >
          <router-link class="text-light" :to="{ name: 'Cart' }">
            <i class="fas fa-cart-arrow-down"></i>
          </router-link>
          <span class="cart-count">{{ cartCount }}</span>
        </div>
      </ul>
    </div>
  </nav>
</template>

<script>
import UserService from "@/services/user.service";
import jwt_decode from "jwt-decode";
export default {
  name: "Navbar",
  props: ["cartCount"],
  data() {
    return {
      token: null,
      isAdmin: false,
    };
  },
  methods: {
    async getUser() {
      const user_id = jwt_decode(this.token);
      try {
        const response = await UserService.get(user_id.id);
        this.isAdmin = response.data.isAdmin;
        console.log(this.isAdmin);
      } catch (error) {
        console.log(error);
      }
    },
    signout() {
      localStorage.removeItem("token");
      this.token = null;
      this.$emit("fetchData");
      this.$emit("resetCartCount");
      this.$router.push({ name: "Home" });
    },
  },
  mounted() {
    this.token = localStorage.getItem("token");
    this.getUser();
  },
};
</script>

<style scoped>
.navbar {
  /* background-color: #fff; */
  font-size: 1.1rem;
  padding: 0px;
  background-color: rgb(94, 63, 179);
  color: white;

  text-align: center;
  display: flex;
  justify-items: center;
}

.my-image {
  border: 0px;
  background-color: transparent;
  width: 70px;
}

.nav {
  padding-right: 90px;
}

.nav-menu {
  list-style-type: none;
  margin: -1px -3px;
  /*  ////////////////////////////////////////////////////////////*/
  padding: 10px;
  right: 50px;
}

.nav-menu li {
  color: rgba(255, 255, 255);
  float: center;
}

.nav-item {
  display: inline-block;
  margin-right: 10px;
  /* width: 1rem; */
}

.nav-link {
  color: black;
  font-size: 18px;
  text-decoration: none;
}

.nav-dropdown {
  display: none;
  position: absolute;
  background-color: #fff;
  padding: 10px;
  z-index: 1000;
}

.nav-dropdown li:hover {
  background-color: rgb(202, 100, 100);
  /* Replace with the desired color */
}

.nav-item:hover .nav-dropdown {
  display: block;
}


.navbar-brand {
  float: left;
  margin-right: 20px;
}

.navbar-brand:hover {
  color: #1a1820;
}

.navbar-search {
  float: right;
  width: 30px;
}

.navbar-cart {
  float: right;
  margin-right: 10px;
  position: relative;
  font-size: 18px;
  /* ///////////////////////////////*/
}

.fa-cart-arrow-down {
  color: #fff;
  font-size: 26px;
  margin-top: 4px;
}

.fa-cart-arrow-down:hover {
  color: #1a1820;
}

.cart-count {
  background-color: #fff;
  color: #e10303;
  border-radius: 108%;
  padding: 0px 3px;
  font-size: 0.9rem;
  margin-left: 2px;
  font-weight: 900;
}

.nav-link-a {
  color: #fff;
  text-decoration: none;
}

.logo {
  color: #fff;
  font-size: 30px;
  padding-left: 10px;
}

.d-flex gap-5 {
  color: #fff;
}

a {
  font-weight: 700;
  color: aliceblue;
}

.text-white {
  text-decoration: none;
}

.login-btn1:hover {
  background: linear-gradient(45deg, #aedbd8, #125092);
}


h1 {
  margin: 7px 4px 10px;
}

.login-btn {
  display: inline-block;
  background-color: rgb(94, 63, 179);
  color: #fff;
  text-decoration: none;
  border-radius: 5px;
  position: relative;
  transition: background-color 0.3s ease;
  border: 1px solid #f3f3f3;
  padding: 11px 17px 5px;
  margin-top: 1px;

}

.login-btn1 {
  display: inline-block;
  padding: 14px 21px;
  background-color: rgb(0 0 0 / 55%);
  color: #fff;
  text-decoration: none;
  border-radius: 5px;
  position: relative;
}

/* Ẩn các thẻ con khi chưa hover */
.login-btn .hidden-content {
  display: none;
  position: absolute;
  top: 100%;
  left: 0;
  background-color: #0f91d3;
  color: #fff;
  padding: 10px;
  border-radius: 10px;
  box-shadow: 0 0 10px #0f91d3
}

/* Hiển thị các thẻ con khi hover vào nút */
.login-btn:hover .hidden-content {
  display: block;
}

/* Loại bỏ gạch chân từ chữ trong các thẻ con */
.login-btn .hidden-content a {
  text-decoration: none;
  color: #fff;
  transition: background-color 0.3s ease;
}

.login-btn .hidden-content a:hover {
  color: #1a1820;
}

</style>
