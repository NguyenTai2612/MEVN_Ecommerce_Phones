<template>
  <div id="home">
    <!-- Page Wrapper -->
    <!-- <div id="background-div" class="page-holder bg-cover">
        </div> -->
    <div class="image-slider">
      <img :src="currentImage" :alt="`Image ${currentImageIndex + 1}`" />
    </div>

    <div class="container">
      <div class="row">
        <div class="col-12 text-left d-flex" style="margin-left: 150px;">


          <div class="p-3 d-flex gap-3">
            <label for="category" class="p-1" style="font-size: larger;height: 10px;width: 60px;"></label>
            <select class="form-select" aria-label="Default select example" id="category" v-model="category" style="padding-right: 20px;border: 2px solid #333;"  
              @click="handleChange">
              <!-- <option selected>Giá</option> -->
              <option value="all">Brand</option>
              <option value="iPhone">iPhone</option>
              <option value="Samsung">Samsung</option>
              <option value="Oppo">Oppo</option>
              <option value="Realme">Realme</option>
              <option value="Vivo">Vivo</option>
            </select>
          </div>

          <form class="d-flex col-6 p-3" role="search" @submit="submitProduct">
            <input class="form-control me-2" type="search" placeholder="Siêu phẩm iPhone 15 Series " aria-label="Search"
              v-model="searchText" style="margin-left: -23px;border: 2px solid #333;" />
            <button @click="search">
              <i class="fas fa-search"></i>
            </button>
          </form>
        </div>
      </div>
      <div class="row">
        <div v-for="index in this.product_size" :key="index"
          class="col-md-6 col-xl-3 col-12 p-4 justify-content-around d-flex">
          <ProductBox :product="productList.data[index - 1]">
            {{ productList.data[index - 1] }}
          </ProductBox>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import ProductBox from "../components/Product/ProductBox.vue";
import InputSearch from "../components/Product/InputSearch.vue";
import ProductService from "@/services/product.service";
// import CategoryBox from "../components/Category/CategoryBox.vue";
export default {
  name: "Home",
  components: {
    ProductBox,
    InputSearch,
  },
  props: ["products"],
  data() {
    return {
      images: [
        "https://manhcuongmobile.vn/wp-content/uploads/2023/10/ip15-pm-230929022330.webp",
        "https://manhcuongmobile.vn/wp-content/uploads/2023/10/15pl-230929022403.webp",
        "https://manhcuongmobile.vn/wp-content/uploads/2023/10/ip15-230929022239.webp",
        // Thêm đường dẫn của các ảnh khác nếu cần
      ],
      currentImageIndex: 0,
      activeIndex: -1,
      searchText: "",
      product_size: 0,
      productList: null,
      category: "all",
      current_page: 1,
    };
  },

  computed: {
    currentImage() {
      return this.images[this.currentImageIndex];
    },
  },

  methods: {
    getAll() {
      this.productList = this.products;
    },
    onPageClick(event) {
      const page = event.target.getAttribute("page");

      // Chuyển sang trang được chọn
      this.current_page = page;
      this.getAll();
    },
    async submitProduct(e) {
      e.preventDefault();
      const filter = this.searchText;
      const response = await ProductService.getAll(filter);

      if (response) {
        this.productList = response;
        // this.productList.data.unshift(response.data);
        this.product_size = this.productList.data.length;
      }

      this.searchText = "";
    },
    async handleChange() {
      const filter = null;
      const response = await ProductService.getAll(filter, this.category);
      console.log(response);
      if (response) {
        this.productList = response;
        this.product_size = this.productList.data.length;
      }

      // this.searchText = ""
      console.log(this.category);
    },
    startAutoplay() {
      setInterval(() => {
        this.currentImageIndex =
          (this.currentImageIndex + 1) % this.images.length;
      }, 2000); // Chuyển ảnh sau mỗi 2 giây
    },
  },
  mounted() {
    this.token = localStorage.getItem("token");
    this.getAll();
    this.startAutoplay();
    this.product_size = this.products.total;
    // console.log(this.product_size)
    console.log(this.products.data);
    console.log(this.searchText);
    console.log(this.category);

    // this.product_size = Math.min(8, this.product_size);
  },
  created() {
    this.token = localStorage.getItem("token");
  },
};
</script>

<style>
.page-holder {
  min-height: 100vh;
}

.bg-cover {
  background-size: cover !important;
}

/* #background-div {
    background: url(../assets/anh.jpg)
} */

#heading {
  text-decoration: none;
  font-family: "Arial", sans-serif;
  font-weight: 400;
  opacity: 0.8;
  font-family: "Arial", sans-serif;
}

#content {
  opacity: 0.8;
}

h2 {
  font-family: "Arial", sans-serif;
}
</style>

<style scoped>
.image-slider {
  width: 100%;
  height: auto;
  overflow: hidden;
}

.image-slider img {
  width: 100%;
  height: auto;
}

.form-select {
  width: 63%;

}

/* Style cho nút tìm kiếm */
button {
  background-color: #3498db; /* Màu nền */
  color: #fff; /* Màu chữ */
  border: none; /* Loại bỏ đường viền */
  padding: 10px 15px; /* Kích thước nút */
  border-radius: 5px; /* Bo tròn góc */
  cursor: pointer; /* Hiển thị icon tìm kiếm khi di chuyển chuột */
}

/* Style khi di chuột vào nút */
button:hover {
  background-color: #2980b9; /* Màu nền khi di chuột vào */
}

</style>
