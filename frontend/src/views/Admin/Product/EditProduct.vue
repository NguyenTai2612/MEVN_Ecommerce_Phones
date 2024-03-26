<template>
    <div v-if="product" class="page">
        <div class="card-header" style="
        padding-bottom: 23px;
        padding-right: 47px;
        font-size: 27px;
        font-weight: 700;
        color: darkblue;
        ">Edit Product
        </div>
        <div class="custom-div">
            <FormProduct :product="product" @submit:product="updateProduct" />
        </div>

    </div>
</template>

<script>
import FormProduct from "@/components/Product/FormProduct.vue";
import CartService from "@/services/cart.service";
import OrderService from "@/services/order.service";
import ProductService from "@/services/product.service";
import jwt_decode from "jwt-decode";
export default {
    components: {
        FormProduct
    },
    data() {
        return {
            token: null,
            product: null,
        }
    },
    props: {
        id: { type: String, required: true },

    },

    methods: {
        // list of order histories
        async getProduct(id) {
            // const user_id = jwt_decode(this.token);
            try {
                const response = await ProductService.get(id);
                this.product = response.data
                console.log(this.product)
            } catch (error) {
                console.log(error);

            }

        },
        async updateProduct(data) {
            // console.log(data)
            console.log(data)
            try {
                const response = await ProductService.update(this.id, data);
                if (response) {
                    this.$router.push({
                        name: "AdminProduct",

                    });
                }
            } catch (error) {
                console.log(error);
            }
        },


    },
    created() {
        // this.token = localStorage.getItem("token");
        this.getProduct(this.id);
    },
}
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

.card {
    --bs-card-border-radius: 0.375rem;
    --bs-card-border-width: 7px;
    --bs-card-spacer-x: -1rem;
    --bs-card-border-color: rgb(0 0 0 / 55%);
}

.custom-div {
    width: unset;
    height: auto;
    border: 1px solid #000;
    padding: 54px 0px 50px;
}

.row {
    --bs-gutter-x: -12.5rem;
}

</style>