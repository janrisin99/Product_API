<script setup>
import { computed, onMounted, ref } from 'vue'

const query = new URLSearchParams(window.location.search)
const activePage = ref(query.get('page') === 'users' ? 'users' : 'products')
const productView = ref(query.get('view') === 'table' ? 'table' : 'cards')
const products = ref([])
const users = ref([])
const productSearch = ref('')
const loadingProducts = ref(false)
const loadingUsers = ref(false)
const productError = ref('')
const userError = ref('')

const productsApi = 'https://dummyjson.com/products?limit=12'
const usersApi = 'https://jsonplaceholder.typicode.com/users'

const filteredProducts = computed(() => {
  const keyword = productSearch.value.trim().toLowerCase()
  if (!keyword) return products.value
  return products.value.filter((product) =>
    `${product.title} ${product.category}`.toLowerCase().includes(keyword),
  )
})

const formatPrice = (value) =>
  Number(value).toLocaleString('en-US', {
    style: 'currency',
    currency: 'USD',
  })

async function loadProducts() {
  loadingProducts.value = true
  productError.value = ''

  try {
    const response = await fetch(productsApi)
    if (!response.ok) throw new Error(`HTTP ${response.status}`)

    const data = await response.json()
    if (!Array.isArray(data.products)) throw new Error('รูปแบบข้อมูลสินค้าไม่ถูกต้อง')
    products.value = data.products
  } catch (error) {
    productError.value = `ไม่สามารถโหลดข้อมูลสินค้าได้: ${error.message}`
  } finally {
    loadingProducts.value = false
  }
}

async function loadUsers() {
  loadingUsers.value = true
  userError.value = ''

  try {
    const response = await fetch(usersApi)
    if (!response.ok) throw new Error(`HTTP ${response.status}`)

    const data = await response.json()
    if (!Array.isArray(data)) throw new Error('รูปแบบข้อมูลผู้ใช้ไม่ถูกต้อง')
    users.value = data
  } catch (error) {
    userError.value = `ไม่สามารถโหลดข้อมูลผู้ใช้ได้: ${error.message}`
  } finally {
    loadingUsers.value = false
  }
}

function showPage(page) {
  activePage.value = page
  if (page === 'users' && users.value.length === 0 && !loadingUsers.value) loadUsers()
}

onMounted(() => {
  window.scrollTo({ top: 0, left: 0 })
  if (activePage.value === 'users') loadUsers()
  else loadProducts()
})
</script>

<template>
  <header class="navbar navbar-dark bg-dark py-3 shadow-sm">
    <div class="container d-flex justify-content-between gap-3 flex-wrap">
      <span class="navbar-brand mb-0 fw-bold">แบบฝึกหัดที่ 17 - Vue.js API</span>
      <nav class="d-flex gap-2" aria-label="เมนูหลัก">
        <button class="btn" :class="activePage === 'products' ? 'btn-warning' : 'btn-outline-light'" @click="showPage('products')">ข้อมูลสินค้า</button>
        <button class="btn" :class="activePage === 'users' ? 'btn-warning' : 'btn-outline-light'" @click="showPage('users')">ข้อมูลผู้ใช้</button>
      </nav>
    </div>
  </header>

  <main class="container my-4">
    <section v-if="activePage === 'products'" aria-labelledby="products-heading">
      <div class="mb-4">
        <span class="badge text-bg-primary mb-2">Products API</span>
        <h1 id="products-heading" class="h3 mb-2">ข้อมูลสินค้าผ่าน API</h1>
        <p class="text-muted mb-1">ดึงข้อมูลสินค้าจาก DummyJSON และเลือกแสดงได้ทั้งแบบคอลัมน์และแบบตาราง</p>
        <small class="text-muted">แหล่งข้อมูล: {{ productsApi }}</small>
      </div>

      <div class="toolbar d-flex gap-3 justify-content-between align-items-center flex-wrap mb-3">
        <div class="btn-group" role="group" aria-label="เลือกรูปแบบการแสดงสินค้า">
          <button class="btn btn-outline-primary" :class="{ active: productView === 'cards' }" @click="productView = 'cards'">แบบคอลัมน์</button>
          <button class="btn btn-outline-primary" :class="{ active: productView === 'table' }" @click="productView = 'table'">แบบตาราง</button>
        </div>
        <div class="d-flex gap-2 search-group">
          <input v-model="productSearch" class="form-control" type="search" placeholder="ค้นหาชื่อหรือหมวดหมู่สินค้า" aria-label="ค้นหาชื่อหรือหมวดหมู่สินค้า" />
          <button class="btn btn-outline-secondary" :disabled="loadingProducts" @click="loadProducts">รีเฟรช</button>
        </div>
      </div>

      <div v-if="loadingProducts" class="alert alert-info" role="status">กำลังโหลดข้อมูลสินค้า...</div>
      <div v-else-if="productError" class="alert alert-danger" role="alert">
        {{ productError }}
        <button class="btn btn-sm btn-outline-danger ms-2" @click="loadProducts">ลองใหม่</button>
      </div>

      <template v-else>
        <p class="small text-muted">แสดง {{ filteredProducts.length }} รายการ</p>
        <div v-if="productView === 'cards'" class="row g-3">
          <article v-for="product in filteredProducts" :key="product.id" class="col-12 col-sm-6 col-lg-4 col-xl-3">
            <div class="card h-100 shadow-sm">
              <img :src="product.thumbnail" :alt="product.title" class="card-img-top product-img" />
              <div class="card-body d-flex flex-column">
                <div class="small text-primary text-capitalize mb-1">{{ product.category }}</div>
                <h2 class="h6 card-title">{{ product.title }}</h2>
                <p class="card-text small text-muted flex-grow-1">{{ product.description }}</p>
                <div class="d-flex align-items-center justify-content-between gap-2">
                  <strong class="text-primary">{{ formatPrice(product.price) }}</strong>
                  <span class="badge text-bg-light">★ {{ product.rating }}</span>
                </div>
              </div>
            </div>
          </article>
        </div>
        <div v-else class="table-responsive shadow-sm rounded">
          <table class="table table-bordered table-striped align-middle mb-0">
            <thead class="table-dark"><tr><th scope="col">ID</th><th scope="col">รูปภาพ</th><th scope="col">ชื่อสินค้า</th><th scope="col">หมวดหมู่</th><th scope="col">ราคา (USD)</th><th scope="col">คะแนน</th></tr></thead>
            <tbody><tr v-for="product in filteredProducts" :key="product.id"><td>{{ product.id }}</td><td><img :src="product.thumbnail" :alt="product.title" class="table-image" /></td><td>{{ product.title }}</td><td class="text-capitalize">{{ product.category }}</td><td>{{ formatPrice(product.price) }}</td><td>★ {{ product.rating }}</td></tr></tbody>
          </table>
        </div>
        <div v-if="filteredProducts.length === 0" class="alert alert-secondary">ไม่พบสินค้าที่ตรงกับคำค้นหา</div>
      </template>
    </section>

    <section v-else aria-labelledby="users-heading">
      <span class="badge text-bg-success mb-2">Users API</span>
      <h1 id="users-heading" class="h3 mb-2">ข้อมูลผู้ใช้ผ่าน API</h1>
      <p class="text-muted">ดึงข้อมูลผู้ใช้จาก JSONPlaceholder และแสดงผลในรูปแบบตาราง</p>
      <div class="d-flex justify-content-between align-items-center gap-3 mb-3 flex-wrap">
        <small class="text-muted">แหล่งข้อมูล: {{ usersApi }}</small>
        <button class="btn btn-outline-secondary" :disabled="loadingUsers" @click="loadUsers">รีเฟรช</button>
      </div>
      <div v-if="loadingUsers" class="alert alert-info" role="status">กำลังโหลดข้อมูลผู้ใช้...</div>
      <div v-else-if="userError" class="alert alert-danger" role="alert">
        {{ userError }}
        <button class="btn btn-sm btn-outline-danger ms-2" @click="loadUsers">ลองใหม่</button>
      </div>
      <div v-else class="table-responsive shadow-sm rounded">
        <table class="table table-striped table-bordered align-middle mb-0">
          <thead class="table-dark"><tr><th scope="col">ID</th><th scope="col">ชื่อ-นามสกุล</th><th scope="col">ชื่อผู้ใช้</th><th scope="col">อีเมล</th><th scope="col">โทรศัพท์</th><th scope="col">เมือง</th></tr></thead>
          <tbody><tr v-for="user in users" :key="user.id"><td>{{ user.id }}</td><td>{{ user.name }}</td><td>{{ user.username }}</td><td><a :href="`mailto:${user.email}`">{{ user.email }}</a></td><td>{{ user.phone }}</td><td>{{ user.address?.city }}</td></tr></tbody>
        </table>
      </div>
    </section>
  </main>

  <footer class="container pb-4 small text-muted">แบบฝึกหัดที่ 17 • Vue 3 + Bootstrap 5 • ข้อมูลจาก Public APIs</footer>
</template>
