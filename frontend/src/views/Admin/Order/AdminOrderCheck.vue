<template>
  <div class="container-fluid m-3 align-self-center">
    <div class="card shadow mb-4">
      <div class="card-header py-3">
        <h6 class="m-0 font-weight-bold">Order Pending</h6>
        <!-- <a type="button" class="addproduct" @click="addProduct">AddOrder</a> -->
      </div>
      <div class="card-body">
        <div class="table-responsive">
          <table class="table table-bordered" id="dataTable" width="100%" cellspacing="0">
            <thead>
              <tr style="color: darkblue;">
                <th>Phone Number</th>
                <th>Name</th>
                <th>Address</th>
                <th>Order Date</th>
                <th>Total Cost</th>
                <th>Order Detail</th>
                <th>Order Confirm</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="order in orderList" :key="order">
                <td>{{ order.user_id.phone }}</td>
                <td>{{ order.user_id.name }}</td>
                <td>{{ order.address }}</td>
                <td>{{ moment(order.createdAt) }}</td>
                <td>{{ order.totalcost }} $</td>
                <td type="button" class="detail bg-gradient-primary collapsed" @click="detailOrder(order._id)">
                  <button type="button" style="background-color: rgb(155, 243, 243);border-radius: 5px; color: black; ">
                    View
                  </button>
                </td>
                <td >
                  <ul class="d-flex gap-3 collapsed " style="margin-left: 18px;">
                    <!-- @click="updateProduct(product._id)"  data-bs-toggle="modal" data-bs-target="#exampleModal"-->
                    <li @click="check(order._id)" style="font-size: 30px;">
                      <i type="button" class="fas fa-check-square" style="color: green"></i>
                    </li>
                    <li @click="deleteOrder(order._id)" style="font-size: 30px;" >
                      <i  type="button" class="fas fa-times" style="color: red"></i>
                    </li>
                  </ul>
                  <!-- <i type="button" class="fas fa-check-square" style="color: green"></i> -->
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
import ProductService from "@/services/product.service";
import jwt_decode from "jwt-decode";
import moment from "moment";

export default {
  data() {
    return {
      token: null,
      orderList: [],
    };
  },
  methods: {
    // list of order histories
    async getProduct() {
      // const user_id = jwt_decode(this.token);
      try {
        const data = false
        const response = await OrderService.getAll(data);
        this.orderList = response.data;
        console.log(response);
      } catch (error) {
        console.log(error);
      }
    },
    async check(id) {
      console.log(id);
      const data = { duyet: true };
      const response = await OrderService.update(id, data);
      console.log(response)
      if (response.status == 'OK') {
        this.getProduct()
      }
    },
    async deleteOrder(id) {
      console.log(id);
      // const data = { duyet: true };
      const response = await OrderService.delete(id);
      console.log(response)
      if (response.status == 'OK') {
        this.getProduct()
      }
    },
    detailOrder(id) {
      this.$router.push({
        name: "DetailOrder",
        params: {
          id,
        },
      });
    },
    moment(value) {
      return moment(value).format("DD/MM/YYYY");
    },
    // async updateProduct(id) {
    //     console.log(id)
    //     this.$router.push({
    //         name: "EditOrder",
    //         params: {
    //             id
    //         },
    //     });
    // },
    // async addProduct() {
    //     this.$router.push({
    //         name: "AddProduct",
    //     });
    // },
  },
  mounted() {
    this.token = localStorage.getItem("token");
    this.getProduct();
  },
  filters: {
    moment(value) {
      return moment(value).format("DD/MM/YYYY");
    },
  },
};
</script>
  
<style scoped>
.avatar {
  width: 30px;
  height: 30px;
}

.text-primary:hover {
  transition: color 0.2s ease-in-out;
  color: green;
}

.addproduct {
  float: right;
}

.detail {
  border: none;
}

.detail :hover {
  text-decoration-color: green;
}

.card {
  --bs-card-border-radius: 0.375rem;
  --bs-card-border-width: 7px;
  --bs-card-spacer-x: -1rem;
  --bs-card-border-color: rgb(0 0 0 / 55%);
}

.m-0 {
  color: darkblue;
  font-size: 30px;
}

table {
  border-collapse: collapse;
  border: 1px solid rgba(2, 2, 2, 0.55);

}

.container, .container-fluid, .container-lg, .container-md, .container-sm, .container-xl, .container-xxl {
    width: 104%;
}

/* .d-flex li i.fas.fa-trash-alt {
  color: red; 
}

.fa-trash-alt:before, .fa-trash-can:before {

    font-size: x-large;
} */
</style>