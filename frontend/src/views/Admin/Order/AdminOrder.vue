<template>
  <div class="container-fluid m-3 align-self-center">
    <div class="card shadow mb-4">
      <div class="card-header py-3">
        <h6 class="m-0 font-weight-bold">Order Processed</h6>
        <!-- <a type="button" class="addproduct" @click="addProduct">AddOrder</a> -->
      </div>
      <div class="card-body">
        <div class="table-responsive">
          <table class="table table-bordered" id="dataTable" width="100%" cellspacing="0">
            <thead>
              <tr style="color: darkblue;" >
                <th>Phone Number</th>
                  <th>Name</th>
                  <th>Address</th>
                  <th>Order Date</th>
                  <th>Total Cost</th>
                  <th>Order Detail</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="order in orderList" :key="order">
                <td>{{ order.user_id.phone }}</td>
                <td>{{ order.user_id.name }}</td>
                <td>{{ order.address }}</td>
                <td>{{ moment(order.createdAt) }}</td>
                <td>{{ order.totalcost }}</td>
                <td type="button" class="detail bg-gradient-primary collapsed" @click="detailOrder(order._id)">
                  <button type="button" style="background-color: rgb(155, 243, 243);border-radius: 5px; color: black; " >
                    View
                  </button>
                </td>
                <!-- <td @click="check(order._id)">
                    <ul class="d-flex gap-3 collapsed text-primary">
                      <li type="button" @click="updateProduct(order._id)">Sửa</li>
                      <li @click="deleteProduct(order._id)">Xóa</li>
                    </ul>
                    <a href="#" type="button">Xác nhận</a>
                  </td> -->
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
        const data = true
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
      // if (response.status == 'OK') {
      //   this.$forceUpdate();
      // }
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
  color: #ff0000;
}

.addproduct {
  float: right;
}

.detail {
  border: none;
}

.detail :hover {
  text-decoration-color: #ff0000;
}

.card {
  --bs-card-border-radius: 0.375rem;
  --bs-card-border-width: 7px;
  --bs-card-spacer-x: -1rem;
  --bs-card-border-color: rgb(0 0 0 / 55%);
}

.m-0{
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

</style>