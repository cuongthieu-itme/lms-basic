<template>
    <Head title="Chào mừng đến với LMS" />
    <div class="min-h-screen bg-gray-50 text-black dark:bg-black dark:text-white transition-colors duration-300 flex flex-col">
        <header class="flex justify-between items-center p-6 max-w-6xl w-full mx-auto">
            <div class="text-2xl font-bold tracking-wide">
                {{ settings?.site_name ?? 'LMS' }} 
            </div>

            <nav class="flex items-center space-x-4">
                <Link
                    v-if="$page.props.auth.user"
                    :href="route('dashboard')"
                    class="px-4 py-2 rounded-md text-black dark:text-white hover:bg-black/10 dark:hover:bg-white/10 transition"
                >
                    Dashboard
                </Link>

                <template v-else>
                    <Link
                        v-if="canLogin"
                        :href="route('login')"
                        class="px-4 py-2 rounded-md text-black dark:text-white hover:bg-black/10 dark:hover:bg-white/10 transition"
                    >
                        Đăng nhập
                    </Link>

                    <Link
                        v-if="canRegister"
                        :href="route('register')"
                        class="px-4 py-2 rounded-md text-black dark:text-white hover:bg-black/10 dark:hover:bg-white/10 transition"
                    >
                        Đăng ký
                    </Link>
                </template>
            </nav>
        </header>

        <main class="flex-grow flex items-center justify-center text-center px-6">
            <div class="max-w-2xl">
                <h1 class="text-4xl md:text-6xl font-extrabold leading-tight mb-6">
                    Học hỏi, tiến bộ và thành công cùng <span class="text-blue-500">LMS</span>.
                </h1>
                <p class="text-lg md:text-xl mb-8 text-black/70 dark:text-white/70">
                    Khám phá một nền tảng đơn giản, trực quan giúp bạn theo dõi khóa học, quản lý bài kiểm tra và nâng cao kỹ năng của mình.
                </p>

                <Link
                    :href="canRegister ? route('register') : (canLogin ? route('login') : '#')"
                    class="inline-block px-8 py-3 text-lg rounded-full bg-blue-500 text-white hover:bg-blue-700 transition ease-in"
                >
                    Bắt đầu ngay
                </Link>
            </div>
        </main>

        <footer class="py-6 text-center text-xs text-black/50 dark:text-white/40 border-t border-black/10 dark:border-white/10">
    <p class="mb-1">
        © 2025 MonLMS — Học tập chưa bao giờ dễ dàng đến thế.
    </p>
</footer>

    </div>
</template>

<script setup>
import { Head, Link } from '@inertiajs/vue3';

defineProps({
    canLogin: Boolean,
    canRegister: Boolean,
    laravelVersion: String,
    phpVersion: String,
    settings: Object,
})
</script>

