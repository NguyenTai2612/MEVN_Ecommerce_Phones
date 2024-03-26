<template>
    <section class="text-center text-lg-start">
        <div class="container py-4">
            <div class="row g-0 align-items-center">
                <div class="col-lg-6 mb-5 mb-lg-0">
                    <div class="card cascading-right" style="
                background: hsla(0, 0%, 100%, 0.55);
                backdrop-filter: blur(30px);
                ">

                        <div class="card-body p-5 shadow-5 text-center" style=" margin-top: -53px;">

                            <h2 class="fw-bold mb-5">Sign up</h2>
                            <form @submit="signup" style="margin-top: -33px;">
                                <div class="form-outline mb-4">
                                    <label class="form-label" for="name">Name Account</label>
                                    <input type="name" class="form-control" v-model="name" required />
                                </div>
                                <div class="mb-4">
                                    <label class="form-label" for="email">Phone number</label>
                                    <input type="phone" class="form-control" v-model="phone" required />
                                </div>
                                <div class="form-outline mb-4">
                                    <label class="form-label" for="pwd">Password</label>
                                    <input type="password" class="form-control" v-model="password" required />
                                </div>
                                <div class="form-group">
                                    <label>Confirm Password</label>
                                    <input type="password" class="form-control" v-model="passwordConfirm" required
                                        style="margin-bottom: 15px;" />
                                </div>

                                <button type="submit" class="btn btn-primary mt-2 py-0" style="
                                    width: 92px;
                                    height: 50px;">
                                    Sign up
                                </button>
                            </form>
                            <p class="small fw-bold mt-2 pt-1 mb-0" style=" margin-left: -3px;">If you already have an
                                account, Please <router-link :to="{ name: 'Signin' }" class="small fw-bold mt-2 pt-1 mb-0"
                                    style="color: red;">
                                    Signin
                                </router-link>
                            </p>
                            <!-- <p>{{message}}</p> -->
                            <!-- Register buttons -->
                            <div class="text-center">
                                <p>or sign up with:</p>
                                <button type="button" class="btn btn-link btn-floating mx-1">
                                    <i class="bi bi-facebook"></i>
                                </button>
                                <button type="button" class="btn btn-link btn-floating mx-1">
                                    <i class="bi bi-google"></i>
                                </button>
                                <button type="button" class="btn btn-link btn-floating mx-1">
                                    <i class="bi bi-twitter"></i>
                                </button>
                            </div>
                        </div>


                        <!-- Submit button -->




                    </div>

                </div>
                <div class="col-lg-6 mb-5 mb-lg-0">
                    <img src="https://png.pngtree.com/png-vector/20220622/ourlarge/pngtree-sign-up-button-click-app-icon-png-image_5254779.png"
                        class="w-60 rounded-4 shadow-4" alt="..." style=" margin-top: -78px;" />
                </div>
            </div>

        </div>

        <!-- Jumbotron -->
    </section>
    <!-- Section: Design Block -->
</template>

<script>

import UserService from "@/services/user.service";
export default {
    name: "Signup",
    props: ["baseURL"],
    data() {
        return {
            name: null,
            phone: null,
            password: null,
            passwordConfirm: null,
        };
    },
    methods: {
        async signup(e) {
            e.preventDefault();
            // if the password matches
            if (this.password === this.passwordConfirm) {
                // make the post body
                const user = {
                    phone: this.phone,
                    name: this.name,
                    password: this.password,
                };

                const response = await UserService.create(user)
                if (response.status == 'OK') {
                    this.$router.push({ name: "Signin" });
                    // console.log(response)
                }

            } else {
                // passwords are not matching
                swal({
                    text: "Error! Passwords are not matching",
                    icon: "error",
                    closeOnClickOutside: false,
                });
            }
        },
    },
};
</script>

<style scoped  >
.cascading-right {
    margin-right: -50px;
}

@media (max-width: 991.98px) {
    .cascading-right {
        margin-right: 0;
    }
}

.p-5 {
    padding: 5rem !important;
}

.cart {
    --bs-card-border-width: 10px;
    --bs-card-border-radius: 1.375rem;
}

.form-control {
    width: 81%;
    margin-left: 57px;
}

p {
    margin-top: 6px;
}
</style>