<template>
    <div class="container">
        <div class="row pt-5 " v-if="product">
            <div class="col-md-1"></div>
            <div class="col-md-4 col-12">
                <img v-if="product" :src="product.image" :alt="product.name" class="img-fluid" />
            </div>
            <div class="col-md-6 col-12 pt-3 pt-md-0">
                <h4>{{ product.name }}</h4>            
                <h6 class="font-weight-bold" style="color: red; font-size: 20px;">$ {{ product.price }}</h6>
             

                <div class="d-flex flex-row ">


                    <div class="quatitly-product">
                        <h5>
                            <div class="">
                            Quantity:
                            <input id="quantity" name="quantity" type="number" v-model="quantity"
                                class=" fs-3" 
                                style="width: 50px;border: 3px solid rgb(51, 51, 51);border-radius: 4px;" />
                                
                                
                            </div>
                        </h5>
                        
                    </div>

                </div>
                <h5 >Type: {{ product.category}}</h5>
                <h5>Description:</h5>
                <p>
                    {{ product.description }}
                </p>

                <div>
                    <div class="input-group col-md-4 col-2 p-0">
                        <button type="button" id="add-to-cart-button" class="btn" @click="addToCart(this.id)">
                            <i class="fas fa-cart-plus"></i>
                            <ion-icon name="cart-outline" v-pre></ion-icon>
                        </button>

                    </div>

                    <div>
                        <router-link to="/">
                            <button type="button" id="back-button" class="btn" style="margin: 3px 1px 3px;">
                                <i class="fas fa-arrow-left"></i>
                            </button>
                        </router-link>
                    </div>

                </div>
                
            </div>
            
        </div>
        <!-- <div v-if="product">
            <Comment  :product="product"/>
        </div> -->
        
    </div>
</template>

<script>
import CartService from "@/services/cart.service";
import ProductService from "@/services/product.service";
import Comment from "@/components/Comment.vue"
import jwt_decode from "jwt-decode";
export default {
    components: {
        Comment
    },
    data() {
        return {
            product: null,
            category: {},
            id: null,
            token: null,
            isAddedToWishlist: false,
            wishlistString: "Add to wishlist",
            quantity: 1,
        };
    },
    props: ["baseURL", "products", "categories"],
    methods: {

        async addToCart(productId) {
            if (!this.token) {
                swal({
                    text: "Please log in first!",
                    icon: "error",
                });
                return;
            }
            const user_id = jwt_decode(this.token);
            // var decoded = jwt_decode(token);    
            const cart = {
                user_id: user_id.id,
                product_id: productId,
                quantity: this.quantity,
                total_price: this.product.price * this.quantity
            };
            await CartService.create(cart)
            this.quantity = 1
            this.$emit("fetchData");
            this.$router.push("/cart");
        },

        listCartItems() {
            if (!this.token) {
                swal({
                    text: "Please log in first!",
                    icon: "error",
                });
                return;
            }
            this.$router.push("/cart");

        },
        async getProduct() {
            this.id = this.$route.params.id;
            // this.product = await ProductService.get(this.id);
            console.log(this.id)
            try {
                const response = await ProductService.get(this.id);
                this.product = response.data
                console.log(this.product)
            } catch (error) {
                console.log(error);

            }
        }
    },
    created() {
        this.getProduct()
    },
    mounted() {
        // this.id = this.$route.params.id;
        // this.getProduct()
        // console.log(this.product.data)
        // this.product = this.products.data.find((product) => product.id == this.id);
        // this.category = this.categories.find(
        //     (category) => category.id == this.product.categoryId
        // );
        this.token = localStorage.getItem("token");
        this.getProduct()
    },
};
</script>

<style scoped>
.category {
    font-weight: 400;
}

.quantity {
    width: 40px;
    text-align: center;
}

/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
}

/* Firefox */
input[type="number"] {
    -moz-appearance: textfield;
}

#add-to-cart-button {
    background-color: #be200b;
    color: white;
}

#back-button {
    background-color: #009719;
    color: white;
}

#wishlist-button {
    background-color: #b9b9b9;
    border-radius: 0;
}

/* #show-cart-button {
    background-color: rgb(211, 190, 0);
    color: white;
} */
</style>