<template>
  <div class="min-h-screen bg-gray-100 flex flex-col">

    <!-- Thanh điều hướng -->
    <nav-bar/>

    <!-- Nội dung chính -->
    <div class="flex flex-col md:flex-row gap-6 p-8">

      <!-- Menu bên -->
      <aside class="bg-white shadow rounded-2xl p-6 w-full md:w-64 space-y-4">
        <h2 class="text-lg font-semibold text-gray-700 mb-2">Quản lý</h2>
        <ul class="space-y-2">
          <li>
            <Link :href="route('course.manage')" class="block px-4 py-2 rounded-lg hover:bg-blue-50 text-gray-700">
              📚 Tạo / Quản lý khóa học
            </Link>
          </li>
          <li>
            <Link :href="route('admin.users')" class="block px-4 py-2 rounded-lg hover:bg-blue-50 text-gray-700">
              🧑‍🤝‍🧑 Quản lý người dùng & vai trò
            </Link>
          </li>
          <li>
            <Link :href="route('quizz.manage')" class="block px-4 py-2 rounded-lg hover:bg-blue-50 text-gray-700">
              📝 Thêm Quiz
            </Link>
          </li>
          <li>
            <Link :href="route('lesson.manage')" class="block px-4 py-2 rounded-lg hover:bg-blue-50 text-gray-700">
        📖    Tạo / Quản lý bài học
            </Link>
          </li>
          <li>
            <Link :href="route('admin.settings.edit')" class="block px-4 py-2 rounded-lg hover:bg-blue-50 text-gray-700">
              ⚙️ Cài đặt chung
            </Link>
          </li>
          <li>
            <Link :href="route('question.manage')" class="block px-4 py-2 rounded-lg hover:bg-blue-50 text-gray-700">
              Quản lý câu hỏi
            </Link>
          </li>
        </ul>
      </aside>

      <!-- Khu vực quản lý chính -->
      <section class="flex-1 bg-white rounded-2xl shadow p-6">
        <h2 class="text-2xl font-bold text-gray-800 mb-4">
          Chào mừng đến trang quản trị
        </h2>
        <p class="text-gray-600 mb-6">
          Chọn một mục trong menu để quản lý nội dung, khóa học, người dùng, quiz và cài đặt.
        </p>

        <!-- Thống kê động -->
        <div class="grid grid-cols-1 sm:grid-cols-3 gap-6">
          <div class="bg-blue-50 p-6 rounded-2xl flex flex-col justify-center items-center shadow-sm">
            <h3 class="text-lg font-semibold text-gray-700 mb-1">Tổng khóa học</h3>
            <p class="text-3xl text-blue-600">{{ stats.totalCourses }}</p>
          </div>
          <div class="bg-green-50 p-6 rounded-2xl flex flex-col justify-center items-center shadow-sm">
            <h3 class="text-lg font-semibold text-gray-700 mb-1">Người dùng</h3>
            <p class="text-3xl text-green-600">{{ stats.totalUsers }}</p>
          </div>
          <div class="bg-yellow-50 p-6 rounded-2xl flex flex-col justify-center items-center shadow-sm">
            <h3 class="text-lg font-semibold text-gray-700 mb-1">Quiz đang hoạt động</h3>
            <p class="text-3xl text-yellow-600">{{ stats.totalQuizzes }}</p>
          </div>
        </div>

      </section>

    </div>

  </div>
</template>

<script setup>
import { Link } from '@inertiajs/vue3';
import { defineProps } from 'vue';
import NavBar from '@/Components/NavBar.vue';

const props = defineProps({
  stats: Object
});
</script>

<style scoped>
body {
  font-family: 'Inter', sans-serif;
}
</style>
