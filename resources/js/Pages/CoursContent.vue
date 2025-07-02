<template>
  <div class="min-h-screen bg-gray-100 p-8 space-y-8">

    <!-- Hình ảnh khóa học -->
    <div class="overflow-hidden rounded-2xl shadow-lg">
      <img :src="course.photo" alt="Hình ảnh khóa học" class="w-full h-64 object-cover" />
    </div>
    <div class="flex justify-center w-full mx-2" v-if="course.instructor">
      <button @click="ModifyCourse()" class="rounded-lg bg-blue-500 hover:bg-blue-600 text-white font-semibold px-4 py-2">
        Chỉnh sửa
      </button>
    </div>

    <!-- Tiêu đề và mô tả -->
    <div class="bg-white p-6 rounded-2xl shadow">
      <h1 class="text-3xl font-bold text-gray-800 mb-2">{{ course.title }}</h1>
      <p class="text-gray-600">{{ course.description }}</p>
    </div>

    <!-- Danh sách bài học -->
    <div class="bg-white p-6 rounded-2xl shadow">
      <h2 class="text-2xl font-semibold text-gray-700 mb-4">Bài học</h2>
      <ul class="space-y-3">
        <li
          v-for="lesson in course.lessons"
          :key="lesson.id"
          class="flex justify-between items-center p-4 rounded-lg shadow-sm border hover:bg-gray-50 transition"
        >
          <span class="text-gray-800 font-medium">{{ lesson.title }}</span>
          <span
            :class="lesson.completed ? 'text-green-600' : 'text-yellow-500'"
            class="font-semibold"
          >
            {{ lesson.completed ? 'Đã hoàn thành' : 'Đang học' }}
          </span>
        </li>
      </ul>
    </div>

    <!-- Nút hành động -->
    <div class="flex justify-center">
      <button
        @click="startOrContinue"
        class="px-6 py-3 bg-blue-600 text-white rounded-full shadow hover:bg-blue-700 transition"
      >
        {{ isCourseCompleted ? 'Ôn lại khóa học' : 'Tiếp tục khóa học' }}
      </button>
    </div>
    <div class="flex justify-center">
      <button
        @click="faireQuizz" v-if="isCourseCompleted "
        class="px-6 py-3 bg-blue-600 text-white rounded-full shadow hover:bg-blue-700 transition"
      >
        Làm bài kiểm tra
      </button>
    </div>

  </div>
</template>

<script setup>
import { computed } from 'vue';
import { router } from '@inertiajs/vue3';

const props = defineProps({
  course: {
    type: Object,
    required: true
  }
});

const isCourseCompleted = computed(() => {
  return props.course.lessons.length > 0 && props.course.lessons.every(lesson => lesson.completed);
});

function startOrContinue() {
  if (isCourseCompleted.value) {
    alert("Bạn đã hoàn thành khóa học này! Bạn có thể ôn lại các bài học.");
  } else {
    const firstIncompleteLesson = props.course.lessons.find(lesson => !lesson.completed);
    if (firstIncompleteLesson) {
      router.visit(`/lesson/${firstIncompleteLesson.id}`);
    } else {
      alert("Không tìm thấy bài học nào.");
    }
  }
}
function ModifyCourse(){
  router.visit(route('course.update',{ course: props.course.id}));
}
function faireQuizz(){
  router.visit(route('quiz.show',{ course: props.course.id, quiz: props.course.firstquiz }));
}
</script>


