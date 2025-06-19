<template>
  <section class="plans-sec py-5" id="plans">
    <div class="container py-md-5 py-3">
      <h5 class="heading mb-2">TIN TỨC</h5>

      <div class="row pricing-plans">
        <div
          class="col-md-4 price-main text-center mb-4"
          v-for="blog in blogs"
          :key="blog.id"
        >
          <router-link
            :to="`/single-blog/${blog.id}`"
            class="text-decoration-none"
          >
            <div class="pricing-grid card h-100">
              <div class="card-body d-flex flex-column">
                <img
                  v-if="blog.image"
                  :src="url + blog.image"
                  class="img-fluid mb-3"
                  alt="blog"
                />
                <p class="flex-grow-1">
                  {{ blog.title }}
                </p>
                <div class="price-button mt-md-3 mt-2">
                  <span class="btn text-uppercase btn-outline-primary">
                    Xem bài viết
                  </span>
                </div>
              </div>
            </div>
          </router-link>
        </div>

        <div class="col-12" v-if="isAdmin">
          <div class="text-center mb-4">
            <router-link to="/add-blog" class="btn btn-primary">
              Add New Blog
            </router-link>
          </div>
        </div>

        <div class="col-lg-12 col-md-12">
          <div class="pagination-area">
            <button
              :disabled="!pagination.prev_page_url"
              @click="fetchBlogs(pagination.current_page - 1)"
            >
              &laquo;
            </button>

            <button
              v-for="page in pagination.last_page"
              :key="page"
              :class="{ active: page === pagination.current_page }"
              @click="fetchBlogs(page)"
            >
              {{ page }}
            </button>

            <button
              :disabled="!pagination.next_page_url"
              @click="fetchBlogs(pagination.current_page + 1)"
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
  import axios from 'axios'
import { BASE_API_URL, BASE_IMAGE_URL } from "@/main";

  export default {
    name: 'Blog',
    data() {
      return {
        blogs: [],
        pagination: {},
        isAdmin: false,
        url: BASE_IMAGE_URL + 'blogs/',
      }
    },
    created() {
      const role = localStorage.getItem('role_id')
      this.isAdmin = role === '1'
      this.fetchBlogs(1)
    },
    methods: {
      async fetchBlogs(page = 1) {
        try {
          const token = localStorage.getItem("token");
          const response = await axios.get(`${BASE_API_URL}/blogs?page=${page}`, {
            headers: {
                Authorization: `Bearer ${token}`,
            },
          })
          this.blogs = response.data.data.data
          this.pagination = {
            current_page: response.data.data.current_page,
            last_page: response.data.data.last_page,
            next_page_url: response.data.data.next_page_url,
            prev_page_url: response.data.data.prev_page_url,
          }
        } catch (error) {
          console.error('Failed to fetch blogs:', error)
        }
      },
      formatDate(dateStr) {
        if (!dateStr) return ''
            const options = { year: 'numeric', month: 'short', day: 'numeric' }
            return new Date(dateStr).toLocaleDateString(undefined, options)
        },
    },
  }
</script>

<style scoped>
  .pagination-area button {
    margin: 5px;
    padding: 5px 10px;
    border: none;
    width: 40px;
  }
  .pagination-area button.active {
    background-color: #007bff;
    color: white;
  }
</style>