<template>
  <div class="min-h-screen bg-gray-100 p-8 space-y-8">

    <div class="bg-white p-6 rounded-2xl shadow text-center">
      <h1 class="text-3xl font-bold text-gray-800 mb-2">{{ lesson.title }}</h1>
      <p class="text-gray-600">{{ lesson.description }}</p>
    </div>

    <div class="bg-white p-6 rounded-2xl shadow space-y-6">
      <!-- Trình phát video HTML5 -->
      <div v-if="lesson.videoUrl" class="w-full max-w-4xl mx-auto rounded-lg overflow-hidden">
        <video
          class="w-full rounded-lg"
          controls
          :src="lesson.videoUrl"
        >
          Trình duyệt của bạn không hỗ trợ phát video.
        </video>
      </div>

      <!-- Nội dung giải thích -->
      <div class="text-gray-700 leading-relaxed">
        <p v-for="(paragraph, index) in lesson.content" :key="index" class="mb-4">
          {{ paragraph }}
        </p>
      </div>
    </div>

    <div class="flex justify-between items-center mt-6">
      <Link :href="route('cours.show',lesson.course_id)" class="text-blue-600 hover:underline">Quay lại khóa học</Link>

      <div class="flex space-x-4">
        <button @click="navigate('prev')" class="px-6 py-2 bg-gray-500 text-white rounded-full shadow hover:bg-gray-600 transition">
          Trước
        </button>
        <button @click="navigate('next')" class="px-6 py-2 bg-blue-600 text-white rounded-full shadow hover:bg-blue-700 transition">
          Tiếp theo
        </button>
      </div>

      <button @click="markAsCompleted"
        class="px-6 py-3 bg-green-600 text-white rounded-full shadow hover:bg-green-700 transition">
        {{ lesson.completed ? 'Ôn lại bài học' : 'Đánh dấu hoàn thành' }}
      </button>
    </div>

  </div>
</template>

<script setup>
import { router,Link } from '@inertiajs/vue3';
import { defineProps } from 'vue';
import Swal from 'sweetalert2';
import Toastify from 'toastify-js';
import "toastify-js/src/toastify.css";

const props = defineProps({
  lesson: Object
});

function markAsCompleted() {
  if (!props.lesson.completed) {
    Swal.fire({
      title: 'Bạn có chắc không?',
      text: "Bạn có muốn đánh dấu bài học này là đã hoàn thành?",
      icon: 'question',
      showCancelButton: true,
      confirmButtonColor: '#10B981',
      cancelButtonColor: '#EF4444',
      confirmButtonText: 'Có, đánh dấu!'
    }).then((result) => {
      if (result.isConfirmed) {
        router.post(`/lesson/${props.lesson.id}/complete`, {}, {
          onSuccess: () => {
            Toastify({
              text: "✅ Bài học đã được đánh dấu hoàn thành!",
              duration: 3000,
              gravity: "top",
              position: "right",
              backgroundColor: "#10B981",
            }).showToast();
          }
        });
      }
    });
  } else {
    Swal.fire({
      icon: 'info',
      title: 'Đã hoàn thành',
      text: 'Bạn đã hoàn thành bài học này rồi.',
      confirmButtonText: 'OK'
    });
  }
}

function navigate(direction) {
  router.get(`/lesson/${props.lesson.id}/navigate/${direction}`);
}
</script>

