<template>
  <div class="min-h-screen bg-gray-100 p-6">
    <!-- Liên kết quay lại -->
    <div class="flex justify-between items-center mb-6">
      <Link :href="route('admin.page')" class="flex items-center text-blue-600 hover:underline">
        <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
        </svg>
        Trở về Dashboard
      </Link>

      <h1 class="text-3xl font-bold text-gray-800">📚 Quản lý khóa học</h1>

      <Link :href="route('course.create')" class="bg-blue-600 text-white px-5 py-2 rounded-full shadow hover:bg-blue-700 transition">
        + Thêm khóa học
      </Link>
    </div>

    <div class="mb-4 flex justify-between items-center">
      <input
        type="text"
        v-model="search"
        class="p-2 border rounded w-1/3 focus:ring focus:ring-blue-300"
        placeholder="🔍 Lọc theo tiêu đề..."
      />
    </div>

    <div class="bg-white p-4 rounded-2xl shadow">
      <transition name="fade" mode="out-in">
        <div v-if="loading" key="loading" class="flex justify-center py-8">
          <svg class="animate-spin h-8 w-8 text-blue-600" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
            <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
            <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8v8z"></path>
          </svg>
        </div>

        <table v-else key="table" class="min-w-full divide-y divide-gray-200">
          <thead class="bg-gray-50">
            <tr>
              <th class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase">Hình ảnh</th>
              <th class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase">Tiêu đề</th>
              <th class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase">Giảng viên</th>
              <th class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase">Ngày tạo</th>
              <th class="px-4 py-3 text-center text-xs font-medium text-gray-500 uppercase">Hành động</th>
            </tr>
          </thead>
          <tbody class="divide-y divide-gray-200">
            <tr v-for="course in paginatedCourses" :key="course.id">
              <td class="px-4 py-3">
                <img :src="`/storage/Photo/${course.photo_path_course}`" alt="Hình ảnh khóa học"
                  class="w-16 h-16 object-cover rounded-lg shadow" />
              </td>
              <td class="px-4 py-3 font-semibold text-gray-700">{{ course.title }}</td>
              <td class="px-4 py-3 text-gray-600">{{ course.instructor.name }}</td>
              <td class="px-4 py-3 text-gray-500">{{ formatDate(course.created_at) }}</td>
              <td class="px-4 py-3 text-center space-x-2">
                <Link :href="route('course.edit', course.id)"
                  class="bg-yellow-500 text-white px-4 py-2 rounded-full shadow hover:bg-yellow-600 transition">
                  Chỉnh sửa
                </Link>
                <button @click="confirmDelete(course.id)"
                  class="bg-red-500 text-white px-4 py-2 rounded-full shadow hover:bg-red-600 transition">
                  Xóa
                </button>
              </td>
            </tr>
            <tr v-if="paginatedCourses.length === 0">
              <td colspan="5" class="text-center text-gray-500 py-6">Không có khóa học phù hợp.</td>
            </tr>
          </tbody>
        </table>
      </transition>

      <div v-if="totalPages > 1" class="flex justify-center mt-4 space-x-2">
        <button
          v-for="page in totalPages"
          :key="page"
          @click="changePage(page)"
          class="px-3 py-1 rounded-full"
          :class="{'bg-blue-600 text-white': page === currentPage, 'bg-gray-200': page !== currentPage}"
        >
          {{ page }}
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { Link, router } from '@inertiajs/vue3'
import Swal from 'sweetalert2'
import toast from 'toastify-js'
import "sweetalert2/dist/sweetalert2.min.css"
import "toastify-js/src/toastify.css"


const props = defineProps({
  courses: Array
})

const search = ref('')
const currentPage = ref(1)
const perPage = 8
const loading = ref(true)

const filteredCourses = computed(() => {
  return props.courses.filter(course =>
    course.title.toLowerCase().includes(search.value.toLowerCase())
  )
})

const totalPages = computed(() => Math.ceil(filteredCourses.value.length / perPage))

const paginatedCourses = computed(() => {
  const start = (currentPage.value - 1) * perPage
  return filteredCourses.value.slice(start, start + perPage)
})

function changePage(page) {
  loading.value = true
  setTimeout(() => {
    currentPage.value = page
    loading.value = false
  }, 300)  // Transition simulée fade
}

function confirmDelete(id) {
  Swal.fire({
    title: 'Bạn có chắc không?',
    text: "Hành động này không thể hoàn tác!",
    icon: 'warning',
    showCancelButton: true,
    confirmButtonText: 'Vâng, xóa!',
    cancelButtonText: 'Hủy'
  }).then((result) => {
    if (result.isConfirmed) {
      router.delete(route('course.destroy', id), {
        onSuccess: () => {
          toast({
            text: "✅ Xóa khóa học thành công!",
            duration: 3000,
            close: true,
            gravity: "top",
            position: "right",
            backgroundColor: "#4CAF50",
          }).showToast()
        }
      })
    }
  })
}

function formatDate(dateString) {
  return new Date(dateString).toLocaleDateString('vi-VN', { year: 'numeric', month: 'long', day: 'numeric' })
}

onMounted(() => {
  setTimeout(() => loading.value = false, 500)
})
</script>

<style scoped>
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
</style>
