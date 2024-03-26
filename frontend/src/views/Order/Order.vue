<template>
  <div class="section">

    <div style="padding-left: 92px; color: darkblue;font-size: 2.0rem; font-weight: 600;margin-left: 400px; ">
      <i class="material-icons" style="font-weight: 700; font-size: 40px; color: #333  ">local_shipping
      </i>
      Order Page


    </div>

    <br>
    <div class="container-sm" v-for="orders in orderList" :key="orders.order._id">
      <!-- row -->

      <div class="row">

        <!-- STORE -->
        <div class="container">
          <div class="left-side" style=" margin-top: -18px;">


            <div id="store" class="col-md-12">
              <!-- store products -->
              <div class="card-body">
                <div class="table-responsive">
                  <table class="table table-bordered" id="dataTable" width="100%" cellspacing="0"
                    style=" border: 3px solid #ccc;">
                    <thead>
                      <tr style="background-color: #ccc;">
                        <th style="">Image</th>
                        <th style="">Name</th>
                        <th>Quantity</th>
                        <th style="">Price</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="item in orders.detail" :key="item">
                        <td>
                          <div>
                            <img class="thumbnail" :src="item.product_id.image" alt="" />
                          </div>
                        </td>
                        <td>{{ item.product_id.name }}</td>
                        <td>{{ item.quantity }}</td>
                        <td>$ {{ item.product_id.price }}</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
              <!-- /product -->

              <!-- /store products -->
            </div>
          </div>

          <div class="right-side">
            <div class="top">
              <!-- <p>Order id: {{ orders.order._id }}</p> -->
              <p  v-if="orders.order.duyet" >Order status: 
                <mark style="background-color: green;" >Shipping</mark> 
              </p>
              <p v-if="!orders.order.duyet" >Order status: 
                <mark style="background-color: yellow;" >Pending</mark>
              </p>
              <p>Order date: {{ currentDateTime }}</p>
              <!-- <p>Người nhận: {{ this.nguoiNhan }}</p> -->
              <!-- <p>Số điện thoại người nhận: {{ this.sdtNguoiNhan }}</p> -->
              <p>Address: {{ orders.order.address }}</p>
              <p style="color: firebrick;font-size: x-large; ">Total: $ {{ orders.order.totalcost }}</p>
              
            </div>
            <hr>
            <div class="add-to-cart">
              <a data-bs-toggle="modal" :data-bs-target="'#exampleModal' + orders.order._id"
                class="primar1-btn order-submit">Change address</a>

              <a data-bs-toggle="modal" :data-bs-target="'#exampleModaldelete' + orders.order._id"
                class="primar-btn order-submit">Cancel order</a>
              <!-- <router-link :to="{ name: 'EditAddress', params: { id: product.id }}" ><a  class="primary-btn order-submit">Thay đổi địa chỉ nhận hàng</a></router-link> -->


            </div>


            <div class="modal fade" :id="'exampleModal' + orders.order._id" tabindex="-1"
              aria-labelledby="exampleModalLabel" aria-hidden="true">
              <div class="modal-dialog">
                <div class="modal-content">
                  <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalLabel">
                      Edit Address
                    </h5>
                    <!-- <h3 class="title">Địa chỉ thanh toán và nhận hàng</h3> -->
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                  </div>
                  <div class="modal-body">
                    <div class="form-group">
                      <input class="input" v-model="address" type="text" name="diaChiNhan" placeholder="Address new" />
                    </div>

                    <div class="modal-footer">
                      <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
                        Exit
                      </button>
                      <button type="button" class="btn btn-primary" @click="updateAddress(orders.order._id)"
                        data-bs-dismiss="modal">
                        Apply
                      </button>
                    </div>
                  </div>
                </div>
              </div>
            </div>

            <div class="modal fade" :id="'exampleModaldelete' + orders.order._id" tabindex="-1"
              aria-labelledby="exampleModalLabel" aria-hidden="true">
              <div class="modal-dialog">
                <div class="modal-content">
                  <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalLabel">
                      Cancel order
                    </h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                  </div>
                  <!-- <div class="modal-body">
                            Bạn chắc chắn 
                        </div> -->
                  <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
                      Exit
                    </button>
                    <button type="button" class="btn btn-primary" @click="deleteOrder({ id: orders.order._id })"
                      data-bs-dismiss="modal">
                      Apply
                    </button>
                  </div>
                </div>
              </div>
            </div>

          </div>

        </div>

        <!-- <i>.</i> -->
        <!-- Modal -->
      </div>
      <hr style="border: 2px solid #000000; ">
      <!-- /container -->
    </div>
  </div>
</template>

<script>
import CartService from "@/services/cart.service";
import OrderService from "@/services/order.service";
import jwt_decode from "jwt-decode";
import moment from 'moment';
// import OrderService from "@/services/order.service";

export default {
  data() {
    return {
      token: null,
      orderList: [],
      address: "",
      id: null,
      currentDateTime: moment().format('MMMM Do YYYY, h:mm:ss a')
    };
  },
  props: ["baseURL"],
  name: "Order",
  methods: {
    // list of order histories
    async listOrders() {
      const user_id = jwt_decode(this.token);
      try {
        const response = await OrderService.getOrder(user_id.id);
        console.log(response);

        this.orderList = response.data;
        // for (const item of this.cartItems) {
        //   this.totalcost += item.total_price
        // }
        this.orderList = response.data.reverse();
        console.log(this.orderList);
      } catch (error) {
        console.log(error);
      }
    },
    async deleteOrder(id) {
      console.log(id.id);
      const response = await OrderService.delete(id.id);
      if (response.status == "OK") {
        this.listOrders();
      }
    },
    async updateAddress(id) {
      const data = { address: this.address };
      console.log(id);
      const response = await OrderService.update(id, data);
      if (response.status == "OK") {
        this.listOrders();
        // console.log(response)
      }
    },
    async check(id) {
      console.log(id);
      const data = { duyet: true };
      const response = await OrderService.update(id, data);
      console.log(response);
      if (response.status === 'OK') {
        // Update the order status in the list
        const updatedOrderIndex = this.orderList.findIndex(order => order._id === id);
        if (updatedOrderIndex !== -1) {
          this.$set(this.orderList, updatedOrderIndex, { ...this.orderList[updatedOrderIndex], status: 'Approved' });
        }
      }
    },

  },
  mounted() {
    this.token = localStorage.getItem("token");
    this.listOrders();
  },

};
</script>

<style scoped>
/* h4, h5 {
    font-family: 'Roboto', sans-serif;
    color: #484848;
    font-weight: 700;
  } */

.embed-responsive .card-img-top {
  object-fit: cover;
}

.center {
  justify-content: center;
}

.container {
  display: flex;
}

.left-side {
  flex: 59%;
  background-color: #fff;
  /* Màu nền của phần bên trái */
  padding: 20px;
  /* Khoảng cách lề bên trong của phần bên trái */
  /* border: 3px solid #333; */
  /* Đường viền của phần bên trái */
  box-sizing: border-box;
  /* Đảm bảo kích thước tính cả padding và border */
  border-radius: 5px;
}

.right-side {
  flex: 15%;
  background-color: #ccc;
  /* Màu nền của phần bên phải */
  padding: 20px;
  /* Khoảng cách lề bên trong của phần bên phải */
  /* border: 3px solid #333; */
  /* Đường viền của phần bên phải */
  box-sizing: border-box;
  /* Đảm bảo kích thước tính cả padding và border */
  /* border-radius: 5px; */
}

.container,
.container-fluid,
.container-lg,
.container-md,
.container-sm,
.container-xl,
.container-xxl {
  --bs-gutter-x: 0.5rem;
}

.primar-btn {
  padding: 10px 4px;
  font-size: 16px;
  background-color: #0f068d;
  color: #ffffff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  text-decoration: none;
  margin-left: 7px;
}

.primar1-btn {
  padding: 10px 2px;
  font-size: 16px;
  background-color: green;
  color: #ffffff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  text-decoration: none;
}

.container2 {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  /* Chia đều không gian cho mỗi cột */
  gap: 20px;
  /* Khoảng cách giữa các cột */
}

.column {
  padding: 20px;
  border: 3px solid #ccc;
  /* Đường viền của mỗi cột */
  box-sizing: border-box;
  /* Đảm bảo kích thước tính cả padding và border */
}

.thumbnail {
  max-width: 50px;
  /* Kích thước tối đa của chiều rộng */
  max-height: 50px;
  /* Kích thước tối đa của chiều cao */
  width: auto;
  /* Tự động tính chiều rộng để giữ tỷ lệ khung hình */
  height: auto;
  /* Tự động tính chiều cao để giữ tỷ lệ khung hình */
}

.h3,
h3 {
  font-size: 1.0rem;
}

p {
  margin: 0 0 14px;
  color: #333;
}
</style>