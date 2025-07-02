<template>
    <div class="p-6 max-w-3xl mx-auto">
      <h1 class="text-2xl font-bold mb-6">Chỉnh sửa câu hỏi</h1>
  
      <form @submit.prevent="submit" class="space-y-4 bg-white p-6 rounded shadow">
        <div>
          <label class="block text-sm font-medium">Nội dung câu hỏi</label>
          <textarea v-model="form.question_text" class="w-full p-2 border rounded" rows="3" />
          <span class="text-red-500 text-sm" v-if="form.errors.question_text">{{ form.errors.question_text }}</span>
        </div>
  
        <div class="grid grid-cols-2 gap-4">
          <div>
            <label class="block text-sm font-medium">Lựa chọn A</label>
            <input v-model="form.option_a" class="w-full p-2 border rounded" />
            <span class="text-red-500 text-sm" v-if="form.errors.option_a">{{ form.errors.option_a }}</span>
          </div>
          <div>
            <label class="block text-sm font-medium">Lựa chọn B</label>
            <input v-model="form.option_b" class="w-full p-2 border rounded" />
            <span class="text-red-500 text-sm" v-if="form.errors.option_b">{{ form.errors.option_b }}</span>
          </div>
          <div>
            <label class="block text-sm font-medium">Lựa chọn C</label>
            <input v-model="form.option_c" class="w-full p-2 border rounded" />
            <span class="text-red-500 text-sm" v-if="form.errors.option_c">{{ form.errors.option_c }}</span>
          </div>
          <div>
            <label class="block text-sm font-medium">Lựa chọn D</label>
            <input v-model="form.option_d" class="w-full p-2 border rounded" />
            <span class="text-red-500 text-sm" v-if="form.errors.option_d">{{ form.errors.option_d }}</span>
          </div>
        </div>
  
        <div>
          <label class="block text-sm font-medium">Đáp án đúng</label>
          <select v-model="form.correct_answer" class="w-full p-2 border rounded">
            <option disabled value="">Chọn đáp án</option>
            <option value="option_a">Lựa chọn A</option>
            <option value="option_b">Lựa chọn B</option>
            <option value="option_c">Lựa chọn C</option>
            <option value="option_d">Lựa chọn D</option>
          </select>
          <span class="text-red-500 text-sm" v-if="form.errors.correct_answer">{{ form.errors.correct_answer }}</span>
        </div>
  
        <div>
          <label class="block text-sm font-medium">Quiz liên kết</label>
          <select v-model="form.quiz_id" class="w-full p-2 border rounded">
            <option disabled value="">Chọn quiz</option>
            <option v-for="quiz in quizzes" :key="quiz.id" :value="quiz.id">{{ quiz.title }}</option>
          </select>
          <span class="text-red-500 text-sm" v-if="form.errors.quiz_id">{{ form.errors.quiz_id }}</span>
        </div>
  
        <div class="text-right">
          <button type="submit" class="bg-green-600 text-white px-4 py-2 rounded hover:bg-green-700">
            Lưu thay đổi
          </button>
        </div>
      </form>
    </div>
  </template>
  
  <script setup>
  import { useForm } from '@inertiajs/vue3'
  import { ref } from 'vue'
  import Swal from 'sweetalert2'
  
  const props = defineProps({
    question: Object,
    quizzes: Array
  })
  
  const form = useForm({
    question_text: props.question.question_text,
    option_a: props.question.option_a,
    option_b: props.question.option_b,
    option_c: props.question.option_c,
    option_d: props.question.option_d,
    correct_answer: props.question.correct_answer,
    quiz_id: props.question.quiz_id,
  })
  
  const submit = () => {
    form.put(route('question.update', props.question.id), {
      onSuccess: () => {
        Swal.fire('Thành công', 'Câu hỏi đã được chỉnh sửa thành công', 'success')
      }
    })
  }
  </script>
  