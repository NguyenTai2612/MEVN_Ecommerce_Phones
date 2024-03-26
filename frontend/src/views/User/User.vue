<template>
    <div class="container" style="margin-top: 66px;" >
        <div class="left">
            <div class="" v-if="infor">
                <div class="col">
                </div>

                <div class="">
                    <div class="">
                        <div class="">
                            <div class="">
                                <!-- <h1>Information</h1> -->
                                <br>
                                <h5>Name: {{ infor.name }}</h5>
                                <h5>Address: {{ infor.address }}</h5>
                                <h5>Phone number: {{ infor.phone }}</h5>
                                <hr>
                            </div>

                        </div>

                        <div>
                            <div class="add-to-cart">
                                <router-link :to="{
                                    name: 'EditUser',
                                    params: { id: infor._id },
                                }">
                                    <a><button class="custom-button">
                                            <i class="fas fa-edit edit-icon" title="Chỉnh sửa thông tin"></i>
                                        </button></a>
                                </router-link>
                            </div>

                            <div class="add-to-cart">
                                <router-link to="/">
                                    <a><button class="custom-button" style="background-color: green;margin-left: 12px;" >
                                            <i class="fas fa-arrow-left"></i>
                                        </button></a>
                                </router-link>
                            </div>
                        </div>


                        <!-- <FormUser :infor="infor" @submit:infor="updateUser" /> -->
                    </div>
                </div>
            </div>

        </div>
        <div class="right">

        </div>
    </div>
</template>

<script>
import FormUser from "@/components/FormUser.vue";
import CartService from "@/services/cart.service";
import OrderService from "@/services/order.service";
import UserService from "@/services/user.service";
import jwt_decode from "jwt-decode";
export default {
    components: {
        FormUser,
    },
    data() {
        return {
            token: null,
            orderList: [],
            infor: null,

        }
    },
    props: ["baseURL"],
    name: 'User',
    methods: {
        // list of order histories
        async getUser() {
            const user_id = jwt_decode(this.token);
            try {
                const response = await UserService.get(user_id.id);

                this.infor = response.data
                console.log(this.infor)
            } catch (error) {
                console.log(error);

            }

        },
        async deleteOrder(id) {
            console.log(id.id)
            const response = await OrderService.delete(id.id);
            // if (response.status == 'OK') {
            //   this.$forceUpdate();
            // }
        },
        async updateAddress(id) {
            const data = { address: this.address }
            console.log(id)
            const response = await OrderService.update(id, data);
            if (response.status == 'OK') {
                this.$router.go(0);
                // console.log(response)
            }
        },


    },
    mounted() {
        this.token = localStorage.getItem("token");
        this.getUser();
    },
};

</script>

<style scoped>
.custom-button {
    padding: 10px 20px;
    font-size: 16px;
    background-color: #3498db;
    color: #ffffff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.custom-button:hover {
    background-color: #2980b9;
}

.container {
    display: flex;
}

/* Phần tử con bên trái chiếm 30% */
.left {
    flex: 0 0 40%;
    background-color: lightblue;
    padding: 20px;
}

/* Phần tử con bên phải chiếm 70% */
.right {
    flex: 0 0 60%;
    background-color: #fff;
    padding: 20px;
}

.add-to-cart {
    display: inline-block;
  }

</style>
