<template>
  <section class="team py-5" id="team">
    <div class="container py-md-5 py-3">
      <div class="title-desc text-center mb-5">
        <h5 class="heading heading1 mb-2">GIỚI THIỆU</h5>
        <h3 class="heading heading1">
          <a href="http://cdit.ptit.edu.vn/giai-phap-quan-ly-truc-ca-thong-minh-ezwork/" target="_blank">ezWork</a>
          là giải pháp quản lý hoạt động trực ca định kỳ của nhân viên tại các tòa nhà chung cư, nhà máy, xí nghiệp, cửa hàng, kho bãi,… với khả năng hỗ trợ đa nghiệp vụ như vệ sinh làm sạch, tuần tra an ninh, bảo vệ, giám sát kỹ thuật điện,
          <a href="https://ezwater.vn/" target="_blank">nước</a>, phòng cháy, kiểm kê tài sản, xuất nhập và tồn kho,…
        </h3>
      </div>

      <div class="row team-grid">
        <div
          class="col-lg-3 col-sm-6 mb-4"
          v-for="service in serviceData.filter(s => s && s.id)"
          :key="service.id"
        >
          <router-link :to="`/single-service/${service.id}`" class="text-decoration-none">
            <div class="box13 h-100">
              <img
                v-if="service.image"
                :src="url + service.image"
                class="img-fluid img-thumbnail w-100 h-100"
                style="object-fit: cover"
                alt="service"
              />
              <div class="box-content d-flex align-items-center justify-content-center text-center">
                <h3 class="title noselect" v-html="service.title"></h3>
              </div>
            </div>
          </router-link>
        </div>

        <div class="col-12 mt-4 text-center" v-if="isAdmin">
          <router-link to="/add-service" class="btn btn-primary">
            Add Service
          </router-link>
        </div>

        <div class="col-12 mt-4 text-center">
          <div class="pagination-area">
            <button
              :disabled="!pagination.prev_page_url"
              @click="fetchServices(pagination.current_page - 1)"
            >
              &laquo;
            </button>
            <button
              v-for="page in pagination.last_page"
              :key="page"
              :class="{ active: page === pagination.current_page }"
              @click="fetchServices(page)"
            >
              {{ page }}
            </button>
            <button
              :disabled="!pagination.next_page_url"
              @click="fetchServices(pagination.current_page + 1)"
            >
              &raquo;
            </button>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import axios from "axios";
import { BASE_API_URL, BASE_IMAGE_URL } from "@/main";

export default {
  name: "ServicesTwo",
  data() {
    return {
      serviceData: [],
      url: BASE_IMAGE_URL + 'services/',
      pagination: {},
      isAdmin: false,
    };
  },
  created() {
    const role = localStorage.getItem('role_id');
    this.isAdmin = role === '1';
    this.fetchServices(1);
  },
  methods: {
    async fetchServices(page = 1) {
      try {
        const token = localStorage.getItem("token");
        const response = await axios.get(`${BASE_API_URL}/service?page=${page}`, {
          headers: {
            Authorization: `Bearer ${token}`,
          },
        });
        this.serviceData = response.data.data;
        this.pagination = {
          current_page: response.data.current_page,
          last_page: response.data.last_page,
          next_page_url: response.data.next_page_url,
          prev_page_url: response.data.prev_page_url,
        };
      } catch (error) {
        console.error("Error fetching service data:", error);
      }
    }
  },
};
</script>


<style scoped>
.single-services-box {
  border: 1px solid #eaeaea;
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 2px 15px rgba(0,0,0,0.05);
  transition: all 0.3s ease;
}
.single-services-box:hover {
  box-shadow: 0 4px 20px rgba(0,0,0,0.1);
}
.read-more-btn {
  color: #007bff;
  font-weight: 500;
  display: inline-flex;
  align-items: center;
  gap: 4px;
}
</style>