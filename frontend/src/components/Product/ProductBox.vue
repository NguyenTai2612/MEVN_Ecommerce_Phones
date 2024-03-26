
<template>
  
    <div class="product-card">
      <div class="product-image">
        <router-link :to="{ name: 'ShowDetails', params: { id: product._id } }" class="product-link">
        <img :src="product.image" alt="Product Image" class="img-fluid" />
      </router-link>
      </div>
      <div class="product-details">
        <h5 class="product-title" >{{ product.name }}</h5>
        <p class="product-price">$ {{ product.price }}</p>
        <p @click="addToCart(product._id)">
          <i class="fas fa-cart-plus"  ></i>
        </p>
      </div>
    </div>
  
</template>

<script>
import CartService from "@/services/cart.service";
import ProductService from "@/services/product.service";
import Comment from "@/components/Comment.vue"
import jwt_decode from "jwt-decode";
import Swal from 'sweetalert2';
export default {
  name: "ProductBox",
  data() {
        return {
            
            token: null,
            isAddedToWishlist: false,
            wishlistString: "Add to wishlist",
            quantity: 1,
        };
    },
  props: ["product"],
  methods: {
    handleMouseOver() {
      this.$refs.productImage.style.transform = 'scale(1.05)';
    },
    handleMouseOut() {
      this.$refs.productImage.style.transform = 'scale(1)';
    },
    async addToCart(id) {
        if (!this.token) {
            Swal.fire({
                text: "Please log in first!",
                icon: "error",
            });
            return;
        }

        const user_id = jwt_decode(this.token);
        const cart = {
            user_id: user_id.id,
            product_id: id,
            quantity: 1,
            total_price: this.product.price 
        };

        try {
            await CartService.create(cart);
            this.$emit("fetchData");

            // Hiển thị thông báo khi sản phẩm được thêm vào giỏ hàng thành công
            Swal.fire({
                text: "Product added to cart successfully!",
                icon: "success",
            });
        } catch (error) {
            // Xử lý lỗi nếu có
            Swal.fire({
                text: "Error adding product to cart. Please try again later.",
                icon: "error",
            });
        }
    },
  },
  mounted() {
      
        this.token = localStorage.getItem("token");
       
    },
};
</script>

<style scoped>
.product-link {
  text-decoration: none;
  color: inherit;
}

.product-card {
  display: flex;
  flex-direction: column;
  border: 3px solid rgb(0 0 0 / 55%);
  border-radius: 8px;
  overflow: hidden;
  transition: box-shadow 0.3s ease;
}

.product-card:hover {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.product-image {
  flex: 1;
  overflow: hidden;
}

.product-image img {
  width: 100%;
  height: auto;
  transition: transform 0.3s ease;
}

.product-card:hover .product-image img {
  transform: scale(1.1);
}

.product-details {
  padding: 15px;
  text-align: center;
}

.product-title {
  font-size: 1.2rem;
  margin: 0;
  color: #333;
}

.product-price {
  font-size: 1.2rem;
  color: red;
  font-family:Arial, Helvetica, sans-serif;
}

.product-price sup {
  font-size: 0.8rem;
  vertical-align: top;
}

.fa-cart-plus::before {
    content: "\f217";
    color: red;
    font-size: x-large;
}

.fas.fa-cart-plus:hover {
  cursor: pointer;
  /* Các hiệu ứng khác khi hover có thể được thêm ở đây */
}

</style>
