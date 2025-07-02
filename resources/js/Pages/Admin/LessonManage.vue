<template>
  <div class="min-h-screen bg-gray-100 p-8">
    <!-- Đầu trang -->
    <div class="mb-6 flex justify-between items-center flex-wrap gap-4">
      <div class="flex items-center gap-2">
        <Link :href="route('admin.page')" class="flex items-center text-blue-600 hover:text-blue-800 transition">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7 7-7" />
          </svg>
          Quay lại
        </Link>
        <h1 class="text-3xl font-bold text-gray-800">Quản lý bài học</h1>
      </div>
      <Link :href="route('lesson.create')" class="px-5 py-2 bg-blue-600 text-white rounded-full shadow hover:bg-blue-700 transition">
        ➕ Tạo bài học
      </Link>
    </div>

    <!-- Thanh lọc -->
    <div class="mb-4">
      <input
        v-model="filter"
        type="text"
        placeholder="🔍 Tìm kiếm bài học..."
        class="w-full md:w-1/3 p-3 rounded-2xl border border-gray-300 focus:ring-2 focus:ring-blue-500 focus:outline-none"
      />
    </div>

    <!-- Bảng -->
    <div class="bg-white rounded-2xl shadow overflow-x-auto">
      <table class="w-full text-left table-auto">
        <thead class="bg-gray-50">
          <tr>
            <th class="p-4 text-gray-600 font-medium">#</th>
            <th class="p-4 text-gray-600 font-medium">Tiêu đề</th>
            <th class="p-4 text-gray-600 font-medium">Khóa học liên kết</th>
            <th class="p-4 text-gray-600 font-medium">Ngày tạo</th>
            <th class="p-4 text-gray-600 font-medium">Hành động</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="lesson in filteredLessons" :key="lesson.id" class="hover:bg-gray-50">
            <td class="p-4">{{ lesson.id }}</td>
            <td class="p-4">{{ lesson.title }}</td>
            <td class="p-4">{{ lesson.course_title }}</td>
            <td class="p-4">{{ new Date(lesson.created_at).toLocaleDateString() }}</td>
            <td class="p-4">
              <Link :href="route('lesson.edit', lesson.id)" class="text-blue-600 hover:text-blue-800">Sửa</Link>
              <button @click="deleteLesson(lesson.id)" class="ml-4 text-red-600 hover:text-red-800">
                Xóa
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- Phân trang -->
    <div class="mt-4 flex justify-center" v-if="lessons.links">
      <Link v-for="link in lessons.links" :key="link.url" :href="link.url || ''" v-html="link.label"
            :class="['px-3 py-1 mx-1 rounded', { 
              'bg-blue-600 text-white': link.active, 
              'bg-gray-200 hover:bg-gray-300': !link.active 
            }]">
      </Link>
    </div>
  </div>
</template>

<script setup>
import { Link } from '@inertiajs/vue3';
import { ref, computed } from 'vue';
import Swal from 'sweetalert2';
import Toastify from 'toastify-js';
import "toastify-js/src/toastify.css";

const props = defineProps({
  lessons: Object
});

const filter = ref('');

const filteredLessons = computed(() => {
  if (!filter.value) return props.lessons.data;
  return props.lessons.data.filter(lesson =>
    lesson.title.toLowerCase().includes(filter.value.toLowerCase()) ||
    lesson.course_title.toLowerCase().includes(filter.value.toLowerCase())
  );
});

function deleteLesson(id) {
  // Hiển thị SweetAlert2 để xác nhận
  Swal.fire({
    title: 'Bạn có chắc chắn?',
    text: "Bạn sẽ không thể hoàn tác hành động này!",
    icon: 'warning',
    showCancelButton: true,
          confirmButtonText: 'Có, xóa!',
            cancelButtonText: 'Hủy',
    reverseButtons: true
  }).then((result) => {
    if (result.isConfirmed) {
      axios.delete(route('lesson.destroy', id))
        .then(response => {
          // Thông báo thành công với Toastify
          Toastify({
            text: "Bài học đã được xóa thành công!",
            duration: 3000,
            close: true,
            gravity: "top",
            position: "right",
            backgroundColor: "#10B981",
          }).showToast();

          // Xóa bài học khỏi danh sách mà không cần tải lại trang
          const index = props.lessons.data.findIndex(lesson => lesson.id === id);
          if (index !== -1) {
            props.lessons.data.splice(index, 1);
          }
        })
        .catch(error => {
          // Thông báo lỗi với Toastify
          Toastify({
            text: "Đã xảy ra lỗi khi xóa bài học.",
            duration: 3000,
            close: true,
            gravity: "top",
            position: "right",
            backgroundColor: "#F87171",
          }).showToast();
        });
    }
  });
}
</script>

<style scoped>
body {
  font-family: 'Inter', sans-serif;
}
</style>
