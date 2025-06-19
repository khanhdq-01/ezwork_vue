<template>
  <!-- banner -->
  <section class="banner-section">
    <div class="banner" id="home" style="box-sizing: border-box;">
      <div class="csslider infinity" id="slider1">
        <ul class="banner_slide">
          <!-- Có slide -->
          <template v-if="slideData.length > 0">
            <li v-for="(slide, index) in slideData" :key="slide.id">
              <img
                v-if="slide.image_path"
                :src="url + slide.image_path"
                alt="banner image"
                class="banner-bg-img"
              />
            </li>
          </template>

          <!-- Không có slide -->
          <template v-else>
            <li class="no-slide-message" style="padding-top: 50px;">
              Không có slide nào được hiển thị.
            </li>
          </template>
        </ul>
      </div>
    </div>

    <!-- Quản lý slide -->
<!-- Quản lý slide -->
<div class="slide-button-wrapper" v-if="isLoggedIn && userRole === 1">
  <router-link to="/slide-list" class="btn btn-primary">
    Quản lý Slide
  </router-link>
</div>

  </section>
</template>

<script>
import axios from "axios"
import { BASE_API_URL, BASE_IMAGE_URL } from "@/main";

export default {
  name: "MainBanner",
  data() {
    return {
      slideData: [],
      url: BASE_IMAGE_URL + "slides/",
      isLoggedIn: !!localStorage.getItem("token"),
      userRole: parseInt(localStorage.getItem("role_id")) || null,
    };
  },
  mounted() {
    this.fetchSlideData();
  },
  methods: {
    async fetchSlideData() {
      const token = localStorage.getItem("token");
      try {
        const response = await axios.get(`${BASE_API_URL}/slide`, {
          headers: {
            Authorization: `Bearer ${token}`,
          },
        });
        this.slideData = Array.isArray(response.data.data)
          ? response.data.data
          : [];
        console.log("Slide data fetched successfully:", this.slideData);
      } catch (error) {
        console.error("Error fetching slide data:", error);
      }
    },
  },
};
</script>

<style scoped>
.banner-section {
  position: relative;
}

/* Khi không có slide, đẩy xuống và hiển thị thông báo */
.no-slide-message {
  text-align: center;
  color: #999;
  font-size: 20px;
  padding: 100px 0; /* đẩy nội dung xuống giữa */
}
.slide-button-wrapper {
  display: flex;
  justify-content: center;
  margin-top: 20px;
  margin-bottom: 40px;
}

.slide-button-wrapper .btn {
  font-size: 16px;
  padding: 10px 20px;
}


</style>