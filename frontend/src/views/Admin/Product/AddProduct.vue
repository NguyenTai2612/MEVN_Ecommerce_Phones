<script>
import FormProduct from "@/components/Product/FormProduct.vue";
import CartService from "@/services/cart.service";
import OrderService from "@/services/order.service";
import ProductService from "@/services/product.service";
export default {
    components: {
        FormProduct,
    },
    data() {
        return {
            product: null,
            message: "",
        };
    },
    methods: {
        async addProduct(data) {

            try {
                console.log(data)
                const response = await ProductService.create(data);
                if (response) {
                    this.$router.push({
                        name: "AdminProduct",

                    });
                }
            } catch (error) {
                console.log(error);
            }
        }
    },
    created() {
        this.product = {
            "name": "",
            "category": "",
            "image": "",
            "description": "",
            "price": 0
        }
        this.message = "";
    },
};
</script>

<template>
    <div class="page">
        <div class="card-header" style="
        padding-bottom: 23px;
        padding-right: 47px;
        font-size: 27px;
        font-weight: 700;
        color: darkblue;
        ">Add Product
    </div>
        <div class="custom-div">
            <FormProduct :product="product" @submit:product="addProduct" />
        </div>

    </div>
</template>

<style>
/* CSS */
/* .custom-div {
      width: 200px; 
      height: 100px; 
      border: 2px solid #000; 
      padding: 10px;

    } */

.custom-div {
    width: unset;
    height: auto;
    border: 14px solid #807d7d;
    padding: 54px 0px 50px;
    border-radius: 10px;
}

.row {
    --bs-gutter-x: -8.5rem;
}

textarea.form-control {
    min-height: calc(6.5em + 0.75rem + 2px);
}

.form-control {
    border: 1px solid #333;
}

.container, .container-fluid, .container-lg, .container-md, .container-sm, .container-xl, .container-xxl {
    width: 88%;
}

.container, .container-fluid, .container-lg, .container-md, .container-sm, .container-xl, .container-xxl {
    width: 104%;
}

</style>
