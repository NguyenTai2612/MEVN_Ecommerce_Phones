<template>
    <div class="section">
        <div class="container">
            <div>
                <form class="row " @submit="summit">
                    <div class="py-5 text-center">
                        <i class="fa fa-credit-card fa-4x" style="color: rgb(68, 68, 75)" aria-hidden="true"></i>
                        <h2 style="color: rgb(71, 71, 77)">Payment </h2>
                        <p class="lead">Please check your Customer information and Shopping Cart information before
                            ordering.
                        </p>
                    </div>

                    <div class="col-md-7" style="padding: 35px;">
                        <div class="billing-details ">
                            <div class="">
                                <p class="title"
                                    style="padding-left: 71px;color: rgb(0, 0, 0);padding-bottom: 23px;font-size: 30px;">
                                    Shipping Address</p>
                            </div>
                            <div class="form-group mb-3">
                                <input class="input" type="text" name="ten" placeholder="Full name"
                                    style="margin-left: 63px;">
                            </div>
                            <div class="form-group mb-3">
                                <input class="input" type="text" name="ten" placeholder="Address" v-model="address" required
                                    style="margin-left: 63px;">
                            </div>
                            <div class="form-group mb-3">
                                <input class="input" type="tel" name="sdt" placeholder="Your Phone"
                                    style="margin-left: 63px;">
                            </div>
                            <div class="form-group">
                                <input class="input" type="text" name="diaChi" placeholder="Email"
                                    style="margin-left: 63px;">
                            </div>
                        </div>
                    </div>

                    <div class="col order-details"
                        style="border: 1px solid rgb(51, 51, 51);margin-left: 600px;margin-right: -93px;margin-top: -425px;">
                        <div class="section-title text-center">
                            <h4 class="title" style="color: #000000">Cart</h4>
                        </div>
                        <div class="order-summary">
                            <div class="order-col">

                            </div>
                            <div class="order-products" v-for="cartItem in cartItems" :key="cartItem._id">

                                <div class="order-col">
                                    <div>
                                        <a href="/product/info/{{this._id}}" style="color: #000000; ">
                                        {{cartItem.product_id.name }}
                                        </a>

                                    </div>
                                    <div style="color: #000000; ">{{   cartItem.quantity }}</div>
                                    <div style="color: #000000; ">{{ cartItem.product_id.price * cartItem.quantity }}</div>
                                   
                                </div>
                                <hr>
                            </div>
                            
                           
                            <div class="order-col">
                                
                                <div style="color: #D10024; font-weight: bold;font-size: 20px;">Total: </div>
                                <div>
                                    <strong class="order-total"> $ {{ this.totalcost }}</strong>
                                </div>
                            </div>
                            <div class="order-col">
                                <div style="color: #000000; ">Payment methods</div>
                                <div class="mb-4 pb-2">
                                    <select class="select" style="margin-right: -20px;color: #000000; ">
                                        <option value="1">Payment on delivery</option>
                                        <!-- <option value="2">Giao hàng tiết kiệm</option>
                                        <option value="3">Chuyển phát nhanh</option> -->
                                    </select>
                                </div>
                            </div>
                        </div>
                        <div style="margin-left: 51px;">
                            <!-- <input class="primary-btn order-submit" value="Đặt hàng" data-bs-toggle="modal"
                                data-bs-target="#exampleModal" style="margin-left:54px" /> -->
                            <button type="button" class="btn btn-primary" @click="submit" data-bs-dismiss="modal">
                                Order now
                            </button>
                            <router-link to="#" @click="goBack">
                                <button type="button" id="back-button" class="btn"
                                    style="margin:15px 109px; padding: 12px 40px;">
                                    <i class="fas fa-arrow-left"></i>
                                </button>
                            </router-link>
                        </div>
                    </div>

                </form>

                <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel"
                    aria-hidden="true">
                    <div class="modal-dialog">
                        <div class="modal-content">
                            <div class="modal-header">
                                <h5 class="modal-title" id="exampleModalLabel">Xác nhận đặt hàng</h5>
                                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                            </div>

                            <div class="modal-footer">
                                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Thoát</button>
                                <button type="button" style="margin-left: 65px;" class="btn btn-primary" @click="submit"
                                    data-bs-dismiss="modal">Đặt
                                    hàng ngay</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

        </div>

    </div>
</template>



<script>

import ProductService from "@/services/product.service";
import CartService from "@/services/cart.service";
import OrderService from "@/services/order.service";
import jwt_decode from "jwt-decode";
export default {
    data() {
        return {
            // stripeAPIToken: process.env.VUE_APP_STRIPETOKEN,
            stripe: '',
            token: null,
            cartItems: [],
            totalcost: 0,
            address: '',
        };
    },

    name: 'Checkout',
    props: ['baseURL'],
    methods: {
        // configureStripe() { },
        async listCartItems() {
            const user_id = jwt_decode(this.token);
            try {
                const response = await CartService.getCartUser(user_id.id);
                this.cartItems = response.data;

                // Reset totalcost to 0 before calculating again
                this.totalcost = 0;

                for (const item of this.cartItems) {
                    this.totalcost += item.total_price;
                }

                console.log(this.cartItems);
            } catch (error) {
                console.log(error);
            }
        },
        async getAll() {
            try {
                const response = await CartService.getCartUser(user_id.id);
                // this.cartItems = response.data
                // console.log(this.cartItems)
                let products = response.data;
                let len = Object.keys(products).length;
                for (let i = 0; i < len; i++)
                    this.checkoutBodyArray.push({
                        // imageUrl: products[i].product.imageURL,
                        // productName: products[i].product.name,
                        quantity: products[i].quantity,
                        total_price: products[i].total_price,
                        productId: products[i].product.id,
                        userId: products[i].userId,
                    });
            } catch (error) {
                console.log(error);

            }
        },
        order() {
            this.$router.push({ name: "Order" });
        },
        async submit(e) {
            e.preventDefault();
            // console.log(this.address)
            const user_id = jwt_decode(this.token);
            const products = []
            let totalcost = 0
            for (const cartItem of this.cartItems) {
                console.log(cartItem)
                totalcost += cartItem.total_price
                const product = {
                    id: cartItem.product_id._id,
                    price: cartItem.total_price,
                    discount: 0,
                    quantity: cartItem.quantity
                }
                products.push(product)
                await CartService.delete(cartItem._id)
            }
            const id = user_id.id
            const address = this.address
            console.log(totalcost)
            const response = await OrderService.create({ id, address, totalcost, products });
            if (response.status == 'OK') {
                console.log(response)
                this.$router.push({ name: "Order" });
            }
        },
        goBack() {
            // Sử dụng router để quay trở lại trang trước đó
            this.$router.go(-1);
        },
    },
    mounted() {
        // get the token
        this.token = localStorage.getItem('token');
        // get all the cart items
        // this.stripe = Stripe(this.stripeAPIToken);
        this.listCartItems();
    },
};
</script>

<style scoped>
.section {
    padding-top: 30px;
    padding-bottom: 30px;
    margin: 10px;
}

.billing-details {
    margin: 10px;
}

.shiping-details {
    margin-bottom: 30px;
}

.section-title {
    position: relative;
    margin-bottom: 30px;
    margin-top: 15px;
}

.section-title .title {
    display: inline-block;
    text-transform: uppercase;
    margin: -48px;
}

/* .order-details {
    position: relative;
    padding: 0px 30px 30px;
    border-right: 1px solid #E4E7ED;
    border-left: 1px solid #E4E7ED;
    border-bottom: 1px solid #E4E7ED;
} */

.order-details:before {
    content: "";
    position: absolute;
    left: -1px;
    right: -1px;
    top: -15px;
    height: 30px;
    border-top: 0px solid #E4E7ED;
    border-left: 0px solid #E4E7ED;
    border-right: 0px solid #E4E7ED;
}

.order-summary {
    margin: 15px 0px;
}


.order-summary .order-col {
    display: table;
    width: 100%;
}

.order-summary .order-col:after {
    content: "";
    display: block;
    clear: both;
}

.order-summary .order-col>div {
    display: table-cell;
    padding: 10px 0px;
}

.order-summary .order-col>div:first-child {
    width: calc(100% - 150px);
}

.order-summary .order-col>div:last-child {
    width: 150px;
    text-align: right;
}

.order-summary .order-col .order-total {
    font-size: 24px;
    color: #D10024;
}

/* .order-details .payment-method {
    margin: 30px 0px;
} */

/* .order-details .order-submit {
    display: block;
    margin-top: 13px;
} */

#back-button {
    background-color: #009719;
    color: white;
    font-size: larger;
}

.primary-btn {
    padding: 16px 39px;
    border-radius: 5px;
    background-color: #2980b9;

}

.btn {
    --bs-btn-padding-x: 2.0rem;
    --bs-btn-padding-y: 0.9rem;
    margin-left: 78px;
}

.py-5 {
    padding-top: 0rem !important;
    padding-bottom: 2rem !important;
}


.col-md-5 {
    padding: 30px;
    border: 2px solid #2980b9;
    /* Màu và độ rộng của đường viền */
    border-radius: 26px;
    /* Độ cong của góc */
    background-color: #fff;

}

.col-md-7[data-v-672d98e7] {

    border: 2px solid #ccc;
    background-color: #ccc;
    width: 54.333333%;

    margin-left: 10px;

}

.mb-3 {
    margin-bottom: 2rem !important;
}

.input {
    height: 38px;
    padding: 0px 18px;
    border: 2px solid #ccc;
    border-radius: 1px;
    width: 75%;
}

.order-col a {
    text-decoration: none;
    /* font-weight: 550; Tô đậm chữ */
}

.btn-primary {
    --bs-btn-bg: #2980b9;
    margin-left: 90px;
}
</style>