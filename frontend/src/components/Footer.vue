<template>
  <footer class="footer">
    <div class="container">
      <div class="footer-content">
        <div class="footer-section about">
          <h2 class="logo">Mobile Store</h2>
          <p>
            Your go-to destination for the latest and greatest mobile phones.
          </p>
          <div class="contact">
            <span><i class="fas fa-map-marker-alt"></i> 123 Main Street, Cityville</span>
            <span><i class="fas fa-envelope"></i> info@mobilestore.com</span>
          </div>
        </div>

        <div class="footer-section links">
          <h2>Quick Links</h2>
          <ul>
            <li><router-link to="/">Home</router-link></li>
            <li><router-link to="/products">Products</router-link></li>
            <li><router-link to="/about">About Us</router-link></li>
            <li><router-link to="/contact">Contact</router-link></li>
          </ul>
        </div>

        <div class="footer-section social">
          <h2>Follow Us</h2>
          <div class="social-icons">
            <a href="#" target="_blank"><i class="fab fa-facebook"></i></a>
            <a href="#" target="_blank"><i class="fab fa-twitter"></i></a>
            <a href="#" target="_blank"><i class="fab fa-instagram"></i></a>
          </div>
        </div>
      </div>
    </div>

    <div class="footer-bottom">
      <p>&copy; 2023 Mobile Store. All rights reserved.</p>
    </div>
  </footer>
</template>

<script>
export default {
    name: "Footer"
}
</script>

<style scoped>
.footer {
  background-color: #333;
  color: #fff;
  padding: 30px 0;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.footer-content {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}

.footer-section {
  flex: 1;
  margin-bottom: 20px;
}

.footer-section h2 {
  font-size: 1.5em;
  margin-bottom: 10px;
}

.links ul {
  list-style: none;
  padding: 0;
}

.links li {
  margin-bottom: 8px;
}

.social-icons a {
  color: #fff;
  font-size: 1.5em;
  margin-right: 15px;
}

.footer-bottom {
  text-align: center;
  padding-top: 20px;
  border-top: 1px solid #9886b8;
}


h1, h2, h3, h4,h5 ,h6 {
    color: #d3a912;
}

a {
    color: #6059cd;
}

</style>
