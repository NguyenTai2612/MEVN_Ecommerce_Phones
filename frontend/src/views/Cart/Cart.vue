<template>
    <div class="container">
        <div class="row">
            <div class="col-12 text-center">
                <div>
                    <img src="https://png.pngtree.com/png-clipart/20191122/original/pngtree-cart-icon-isolated-on-abstract-background-png-image_5165752.jpg"
                        style="width: 55px;">
                    <h3 class="pt-3" style="color: darkblue; font-size: 2.0rem;">Shopping Cart </h3>
                </div>
            </div>
        </div>

        <div class="container-1" style="margin: 26px 0px;">
            <div class="left-column">
                <div v-for="cartItem in cartItems" :key="cartItem._id" class="row mt-2 pt-3 justify-content-around">
                    <div class="col-2"></div>
                    <!-- display image -->
                    <div class="col-md-3 embed-responsive embed-responsive-16by9">
                        <router-link :to="{ name: 'ShowDetails', params: { id: cartItem.product_id._id } }">
                            <img v-bind:src="cartItem.product_id.image" class="w-100 card-img-top embed-responsive-item" />
                        </router-link>
                    </div>
                    <!-- display product_id name, quantity and price -->
                    <div class="col-md-5 px-3">
                        <div class="card-block px-3">
                            <h6 class="card-title" style="color: black;">
                                <router-link style="text-decoration: none;"
                                    :to="{ name: 'ShowDetails', params: { id: cartItem.product_id._id } }">{{
                                        cartItem.product_id.name }}
                                </router-link>
                            </h6>

                            <p id="item-price" class="mb-0 font-weight-bold">
                                $ {{ cartItem.product_id.price }}
                            </p>
                            <p id="item-quantity" class="mb-0" style="color: black;">
                                qty:
                                <button @click="decrementQuantity(cartItem)" class="btn btn-sm btn-secondary">-</button>
                                <input size="1" class="p-0 h-25" style="width: 30px;" v-model="cartItem.quantity" />
                                <button @click="incrementQuantity(cartItem)" class="btn btn-sm btn-secondary">+</button>
                                <!-- <button @click="updateQuantity(cartItem)" class="btn btn-sm btn-primary">Cập nhật</button> -->
                            </p>

                            <!-- <p id="item-total-price" class="mb-0">
                        Total Price:
                        <span class="font-weight-bold">
                            $ {{ cartItem.product_id.price * cartItem.quantity }}</span>
                    </p> -->
                            <br />
                            <br>
                            <!-- <a href="#" class="text-right text-danger" @click="deleteItem(cartItem._id)">Delete</a> -->
                            <button type="button" id="del-to-cart-button" class="btn" @click="deleteItem(cartItem._id)">
                                <i class="fas fa-trash"></i>
                            </button>

                            <!-- <div class="form-check">
                        <input class="form-check-input" type="checkbox" value="" id="flexCheckDefault">
                        
                    </div> -->
                        </div>
                    </div>
                    <div class="col-2"></div>
                    <div class="col-12">
                        <hr style="border: 1px solid black;" />
                    </div>
                </div>
            </div>

            <div class="right-column" style="border-left: 5px solid rgb(0 0 0 / 55%);">
                <!-- display total price -->
                <br>
                <div class="" style="margin-right: 1rem;">
                    <h5 style="color: #ac0d0d;">Total: $ {{ totalCost.toFixed() }}</h5>
                    <br>
                    <router-link to="#" @click="goBack">
                        <button type="button" id="back-button" class="btn"
                            style="margin: 11px -11px; width: 62px;padding-bottom: 17px;">
                            <i class="fas fa-arrow-left"></i>
                        </button>
                    </router-link>

                    <button :disabled="isDisabled()" type="button" @click="checkout" class="btn btn-primary confirm">
                        <i class="far fa-credit-card"></i>
                    </button>


                </div>
            </div>


        </div>






    </div>
</template>

<script>
// const axios = require('axios');
import CartService from "@/services/cart.service";
import OrderService from "@/services/order.service";
import jwt_decode from "jwt-decode";
export default {
    data() {
        return {
            cartItems: [],
            token: null,
            totalCost: 0,
        };
    },
    name: 'Cart',
    props: ['baseURL'],
    methods: {
        isDisabled() {
            return this.cartItems.length === 0;
        },
        async listCartItems() {
            const user_id = jwt_decode(this.token);
            try {
                const response = await CartService.getCartUser(user_id.id);
                this.cartItems = response.data.reverse();
                this.updateTotalCost();
                console.log(this.cartItems);
            } catch (error) {
                console.log(error);
            }
        },
        checkout() {
            this.$router.push({ name: 'Checkout' });
        },
        async deleteItem(itemId) {
            const response = await CartService.delete(itemId);
            if (response.status === 'OK') {
                this.listCartItems();
            }
        },
        async order() {
            const user_id = jwt_decode(this.token);
            const products = [];

            for (const cartItem of this.cartItems) {
                const product = {
                    id: cartItem.product_id._id,
                    price: cartItem.total_price,
                    discount: 0,
                    quantity: cartItem.quantity,
                };
                products.push(product);
                await CartService.delete(cartItem._id);
            }

            const id = user_id.id;
            const response = await OrderService.create({ id, products });
            if (response.status === 'OK') {
                console.log(response);
                // this.$router.go(0);
            }
        },
        showDetails(productId) {
            this.$router.push({
                name: 'ShowDetails',
                params: { id: productId },
            });
        },
        updateTotalCost() {
            this.totalCost = this.cartItems.reduce((total, cartItem) => {
                return total + (cartItem.product_id.price * cartItem.quantity);
            }, 0);
        },
        goBack() {
            this.$router.go(-1);
        },
        decrementQuantity(cartItem) {
            if (cartItem.quantity > 1) {
                cartItem.quantity--;
                this.updateTotalCost();
                this.updateCartItemQuantity(cartItem);
            }
        },
        incrementQuantity(cartItem) {
            cartItem.quantity++;
            this.updateTotalCost();
            this.updateCartItemQuantity(cartItem);
        },
        updateQuantity(cartItem) {
            if (cartItem.quantity < 1) {
                cartItem.quantity = 1;
            }
            this.updateTotalCost();
            this.updateCartItemQuantity(cartItem);
        },
        async updateCartItemQuantity(cartItem) {
            try {
                const response = await CartService.update(cartItem._id, { quantity: cartItem.quantity });
                if (response.status === 'OK') {
                    console.log('Quantity updated successfully');
                }
            } catch (error) {
                console.error('Error updating quantity:', error);
            }
        },
    },
    mounted() {
        this.token = localStorage.getItem('token');
        this.listCartItems();
        this.updateTotalCost();
    },
};

</script>

<style scoped>
h4,
h5 {
    font-family: 'Roboto', sans-serif;
    color: #484848;
    font-weight: 700;
}

.embed-responsive .card-img-top {
    object-fit: cover;
}

#item-price {
    color: #232f3e;
}

#item-quantity {
    float: left;
}

#item-total-price {
    float: right;
}

.confirm {
    font-weight: bold;
    font-size: x-large;
    padding-left: 23px;
    margin-left: 31px;
    padding-bottom: 5px;
}

#del-to-cart-button {
    background-color: #be200b;
    color: white;
    margin-left: 240px;
}

#back-button {
    background-color: #009719;
    color: white;
    margin: 27px 1px;
    width: 59px;
    padding-bottom: 15px;
}

.row {
    --bs-gutter-x: 5rem;
}

.container-1 {
    display: flex;
    flex-wrap: wrap;
    margin: 26px 0px;
    border: 5px solid rgb(0 0 0 / 55%);
    border-radius: 7px;
}

.left-column,
.right-column {
    box-sizing: border-box;
    padding: 20px;
    border: 1px solid #ccc;
}

.left-column {
    flex: 1;
    background-color: #fff;
}

.right-column {
    flex: 0 0 200px;
    /* Kích thước cố định của right-column, có thể điều chỉnh theo ý muốn */
    background-color: #ccc;
}

@media (max-width: 768px) {
    .container {
        flex-direction: column;
    }

    .right-column {
        flex: 1;
        margin-top: 20px;
        /* Khoảng cách giữa hai cột khi chúng chuyển xuống dạng dọc */
    }
}

.container,
.container-fluid,
.container-lg,
.container-md,
.container-sm,
.container-xl,
.container-xxl {
    --bs-gutter-x: 14.5rem;
}

.p-0 {
    padding: 1 !important;
}
</style>