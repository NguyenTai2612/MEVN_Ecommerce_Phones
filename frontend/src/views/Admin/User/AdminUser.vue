<template>
    <div class="container-fluid m-3 align-self-center">
                    <div class="card shadow mb-4">
                        <div class="card-header py-3">
                            <h6 class="m-1 font-weight-bold ">User Management</h6>
                        </div>
                        <div class="card-body">
                            <div class="table-responsive">
                                <table class="table table-bordered user" id="dataTable" width="100%" cellspacing="0">
                                    <thead>
                                        <tr style="color: darkblue;" >
                                            <!-- <th>Avatar</th> -->
                                            <th>Name</th>
                                            <th>Phone number</th>
                                            <th>Address</th>
                                            <th>Edit </th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="user in userList" :key="user">
                                            <!-- <td>
                                                <div >
                                                    <img class="avatar" :src="user.avatar" alt="">
                                                </div>
                                            </td> -->
                                            <td>{{ user.name }}</td>
                                            <td>{{ user.phone }}</td>
                                            <td>{{ user.address }}</td>    
                                            <td>
                                                <ul class="delete-icon">
                                                    <li type="button" @click="deleteUser(user._id)">
                                                        <i class="fas fa-trash-alt"></i>
                                                    </li>
                                                </ul>
                                            </td>                                      
                                        </tr>
                                    </tbody>                              
                                                                   
                                </table>
                                                
                            </div>
                        </div>
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
    data() {
        return {
            token: null,
            userList: [],
        }
    },
    name: 'AdminUser',
    methods: {
        // list of order histories
        async getUser() {
            // const user_id = jwt_decode(this.token);
            try {
                const response = await UserService.getAll();
                this.userList = response.data
                console.log(this.userList)
            } catch (error) {
                console.log(error);

            }

        },
        async deleteUser(id) {
            console.log(id)
            const response = await UserService.delete(id);
            if (response.status == 'OK') {
            //   this.$router.push({ name: "AdminUser" });
              this.getUser()
            }
        },
        async updateUser(id) {
            this.$router.push({
                name: "EditUser",
                params: {
                    id
                },
            });
        },


    },
    mounted() {
        this.token = localStorage.getItem("token");
        this.getUser();
    },
}
</script>

<style scoped>
.avatar{
    width: 30px;
    height: 30px;
}

.user{
    text-align: center;
}

.card {
  --bs-card-border-radius: 0.375rem;
  --bs-card-border-width: 7px;
  --bs-card-spacer-x: -1rem;
  --bs-card-border-color: rgb(0 0 0 / 55%);
}

table {
  border-collapse: collapse; 
  border: 1px solid rgba(2, 2, 2, 0.55);

}

.m-1 {
    color: darkblue;
    font-size: 30px;
}



.delete-icon{
  color: #ff0000; /* Màu khi hover */
  font-size: x-large;
}

.container, .container-fluid, .container-lg, .container-md, .container-sm, .container-xl, .container-xxl {
    width: 104%;
}

</style>