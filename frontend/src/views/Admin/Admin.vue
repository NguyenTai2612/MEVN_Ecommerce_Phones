<template>
    <div class="container">
        <nav>
            <ul>
                <li><a href="#" class="logo">
                        <img src="https://www.shutterstock.com/image-vector/business-dashboard-icon-logo-isolated-600nw-2271911029.jpg"
                            alt="">
                        <span class="nav-item">DashBoard</span>
                    </a></li>
                <li>
                    <router-link :to="{ name: 'AdminUser' }">
                        <a class="nav-link collapsed" href="#" data-toggle="collapse" data-target="#collapsePages"
                            aria-expanded="true" aria-controls="collapsePages">
                            <i class="fas fa-user"></i>
                            <span class="nav-item">User Manager</span>
                        </a>
                    </router-link>
                </li>
                <li>
                    <router-link :to="{ name: 'AdminProduct' }">
                        <a class="nav-link collapsed" href="#" data-toggle="collapse" data-target="#collapsePages"
                            aria-expanded="true" aria-controls="collapsePages">
                            <i class="fas fa-box"></i>
                            <span class="nav-item">Product Manager</span>
                        </a>
                    </router-link>
                </li>
                <li>
                    <router-link :to="{ name: 'AdminOrderCheck' }">
                        <a class="nav-link collapsed" href="#" data-toggle="collapse" data-target="#collapsePages"
                            aria-expanded="true" aria-controls="collapsePages">
                            <i class="fas fa-hourglass-half"></i>
                            <span class="nav-item">Order Pending</span>
                        </a>
                    </router-link>
                </li>
                <li>
                    <router-link :to="{ name: 'AdminOrder' }">
                        <a class="nav-link collapsed" href="#" data-toggle="collapse" data-target="#collapsePages"
                            aria-expanded="true" aria-controls="collapsePages">
                            <i class="fas fa-truck"></i>
                            <span class="nav-item">Order Processed</span>
                        </a>
                    </router-link>
                </li>
                <li>
                    <div>
                        <a href="#" @click="signout" class="logout">
                            <i class="fas fa-sign-out-alt"></i>
                            <span class="nav-item">Log out</span>
                        </a>
                        

                    </div>
                </li>
            </ul>
        </nav>

        <section class="main">

            <div class="mt-n1">
                <div class="p-10"></div>
                <div class="container-sm text-center">
                    <div class="row justify-content-md-center">



                        <div class="col-sm-10">
                            <router-view></router-view>
                        </div>
                    </div>
                </div>
            </div>

            <section class="main-course">

            </section>
        </section>
    </div>
</template>


<script>
import Table from "../../components/Table/Table.vue";
export default {
    name: "Admin",
    components: {
        Table,
    },
    data() {
        return {};
    },
    mounted() {
        var token = localStorage.getItem("token");
        // if (!token) {
        //     this.$router.push({ name: "Login" })
        // }
    },
    methods: {
        // logout() {
        //     localStorage.setItem("token", "");
        //     this.$router.push({ name: "Login" });
        // },
        signout() {
            localStorage.removeItem("token");
            this.token = null;
            this.$emit("fetchData");
            this.$emit("resetCartCount");
            this.$router.push({ name: "Home" });
        },
    },
};
</script>



<style scoped>
* {
    margin: 0;
    padding: 0;
    outline: none;
    border: none;
    text-decoration: none;
    box-sizing: border-box;
    font-family: verdana, geneva, sans-serif;
}

body {
    background: #dfe9f5;
}

.container {
    display: flex;
}

nav {
    position: relative;
    top: 0;
    bottom: 0;
    height: 100vh;
    left: 0;
    background: #fff;
    width: 280px;
    overflow: hidden;
    box-shadow: 0 20px 35px rgba(0, 0, 0, 0.1);
}

.logo {
    text-align: center;
    display: flex;
    margin: 10px 0 0 10px;
}

.logo img {
    width: 45px;
    height: 45px;
    border-radius: 50%;
}

.logo span {
    font-weight: bold;
    padding-left: 15px;
    font-size: 18px;
    text-transform: uppercase;
}

a {
    position: relative;
    color: rgb(253 251 251);
    font-size: 14px;
    display: table;
    width: 280px;
    padding: 10px;
}

nav .fas {
    position: relative;
    width: 70px;
    height: 40px;
    top: 14px;
    font-size: 20px;
    text-align: center;
}

.nav-item {
    position: relative;
    top: 12px;
    margin-left: 10px;
}

nav {
    height: 145vh;
    width: 351px;
    background: #000;
}

a:hover {
    background: #696565;
}

/* .logout {
position: absolute;
bottom: 0;
} */

/* Main Section */
.main {
    position: relative;
    padding: 20px;
    width: 100%;
}

.main-top {
    display: flex;
    width: 100%;
}

.main-top i {
    position: absolute;
    right: 0;
    margin: 10px 30px;
    color: rgb(110, 109, 109);
    cursor: pointer;
}

.main-skills {
    display: flex;
    margin-top: 20px;
}

.main-skills .card {
    width: 25%;
    margin: 10px;
    background: #fff;
    text-align: center;
    border-radius: 20px;
    padding: 10px;
    box-shadow: 0 20px 35px rgba(0, 0, 0, 0.1);
}

.main-skills .card h3 {
    margin: 10px;
    text-transform: capitalize;
}

.main-skills .card p {
    font-size: 12px;
}

.main-skills .card button {
    background: orangered;
    color: #fff;
    padding: 7px 15px;
    border-radius: 10px;
    margin-top: 15px;
    cursor: pointer;
}

.main-skills .card button:hover {
    background: rgba(223, 70, 15, 0.856);
}

.main-skills .card i {
    font-size: 22px;
    padding: 10px;
}

/* Courses */
.main-course {
    margin-top: 20px;
    text-transform: capitalize;
}

.course-box {
    width: 100%;
    height: 300px;
    padding: 10px 10px 30px 10px;
    margin-top: 10px;
    background: #fff;
    border-radius: 10px;
    box-shadow: 0 20px 35px rgba(0, 0, 0, 0.1);
}

.course-box ul {
    list-style: none;
    display: flex;
}

.course-box ul li {
    margin: 10px;
    color: gray;
    cursor: pointer;
}

.course-box ul .active {
    color: #000;
    border-bottom: 1px solid #000;
}

.course-box .course {
    display: flex;
}

.box {
    width: 33%;
    padding: 10px;
    margin: 10px;
    border-radius: 10px;
    background: rgb(235, 233, 233);
    box-shadow: 0 20px 35px rgba(0, 0, 0, 0.1);
}

.box p {
    font-size: 12px;
    margin-top: 5px;
}

.box button {
    background: #000;
    color: #fff;
    padding: 7px 10px;
    border-radius: 10px;
    margin-top: 3rem;
    cursor: pointer;
}

.box button:hover {
    background: rgba(0, 0, 0, 0.842);
}

.box i {
    font-size: 7rem;
    float: right;
    margin: -20px 20px 20px 0;
}

.html {
    color: rgb(25, 94, 54);
}

.css {
    color: rgb(104, 179, 35);
}

.js {
    color: rgb(28, 98, 179);
}

.m-3 {
    margin: 2rem !important;
}

fluid,
.container-lg,
.container-md,
.container-sm,
.container-xl,
.container-xxl {

    width: 109%;
}</style>