<script setup>
import { ref, onMounted } from 'vue'
import { Link } from '@inertiajs/vue3'
import ApplicationLogo from '@/Components/ApplicationLogo.vue'

const theme = ref(localStorage.getItem('theme') || 'light')
const mobileMenuOpen = ref(false)

const toggleTheme = () => {
    theme.value = theme.value === 'light' ? 'dark' : 'light'
    localStorage.setItem('theme', theme.value)
    applyTheme()
}

const applyTheme = () => {
    if (theme.value === 'dark') {
        document.documentElement.classList.add('dark')
    } else {
        document.documentElement.classList.remove('dark')
    }
}

const toggleMobileMenu = () => {
    mobileMenuOpen.value = !mobileMenuOpen.value
}

onMounted(() => {
    applyTheme()
})

// Navigation items
const navigation = [
    { name: 'Home', href: route('welcome'), current: route().current('welcome') },
    { name: 'Dashboard', href: route('dashboard'), current: route().current('dashboard') },
    { name: 'Sermons', href: '#', current: false },
    { name: 'Events', href: '#', current: false },
    { name: 'Ministries', href: '#', current: false },
    { name: 'Contact', href: '#', current: false },
]
</script>

<template>
    <nav class="fixed w-full z-50 bg-white/90 dark:bg-gray-900/90 backdrop-blur-sm border-b border-gray-200 dark:border-gray-800 shadow-sm">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between h-20">
                <!-- Logo and brand -->
                <div class="flex items-center">
                    <div class="flex-shrink-0 flex items-center">
                        <Link :href="route('welcome')">
                            <ApplicationLogo class="h-10 w-auto" />
                        </Link>
                        <div class="ml-3">
                            <Link :href="route('welcome')" class="text-xl font-bold text-gray-900 dark:text-white hover:text-indigo-600 dark:hover:text-indigo-400 transition-colors">
                                Global Ministries
                            </Link>
                            <p class="text-xs text-gray-500 dark:text-gray-400">Daily Bread Fellowship</p>
                        </div>
                    </div>

                    <!-- Desktop navigation -->
                    <div class="hidden md:ml-10 md:flex md:space-x-1">
                        <a
                            v-for="item in navigation"
                            :key="item.name"
                            :href="item.href"
                            :class="[
                                item.current
                                ? 'bg-gradient-to-r from-indigo-500 to-blue-500 text-white'
                                : 'text-gray-700 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-800',
                                'px-4 py-2 rounded-lg text-sm font-medium transition-all duration-200'
                            ]"
                        >
                            {{ item.name }}
                        </a>
                    </div>
                </div>

                <!-- Right section -->
                <div class="flex items-center space-x-4">
                    <!-- Theme toggle -->
                    <button
                        @click="toggleTheme"
                        class="p-2 rounded-lg text-gray-500 hover:text-gray-900 dark:text-gray-400 dark:hover:text-white hover:bg-gray-100 dark:hover:bg-gray-800 transition-colors"
                        aria-label="Toggle theme"
                    >
                        <svg v-if="theme === 'dark'" class="h-5 w-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z" />
                        </svg>
                        <svg v-else class="h-5 w-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z" />
                        </svg>
                    </button>

                    <!-- Search (desktop) -->
                    <div class="hidden md:block">
                        <div class="relative">
                            <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                                <svg class="h-5 w-5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
                                </svg>
                            </div>
                            <input
                                type="search"
                                placeholder="Search..."
                                class="pl-10 pr-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg bg-white dark:bg-gray-800 text-gray-900 dark:text-white placeholder-gray-500 dark:placeholder-gray-400 focus:ring-2 focus:ring-indigo-500 focus:border-transparent w-48 lg:w-64 transition-colors"
                            />
                        </div>
                    </div>

                    <!-- Auth buttons (desktop) -->
                    <div class="hidden md:flex items-center space-x-3">
                        <Link
                            :href="route('login')"
                            class="text-gray-700 dark:text-gray-300 hover:text-indigo-600 dark:hover:text-indigo-400 px-4 py-2 text-sm font-medium hover:bg-gray-100 dark:hover:bg-gray-800 rounded-lg transition-colors"
                        >
                            Log in
                        </Link>
                        <Link
                            :href="route('register')"
                            class="bg-gradient-to-r from-indigo-600 to-blue-600 hover:from-indigo-700 hover:to-blue-700 text-white px-4 py-2 rounded-lg text-sm font-medium shadow-lg hover:shadow-xl transform hover:-translate-y-0.5 transition-all duration-200"
                        >
                            Get Started
                        </Link>
                    </div>

                    <!-- Mobile menu button -->
                    <button
                        @click="toggleMobileMenu"
                        class="md:hidden p-2 rounded-lg text-gray-500 hover:text-gray-900 dark:text-gray-400 dark:hover:text-white hover:bg-gray-100 dark:hover:bg-gray-800"
                        aria-label="Toggle menu"
                    >
                        <svg v-if="!mobileMenuOpen" class="h-6 w-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
                        </svg>
                        <svg v-else class="h-6 w-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                        </svg>
                    </button>
                </div>
            </div>
        </div>

        <!-- Mobile menu -->
        <div
            v-if="mobileMenuOpen"
            class="md:hidden bg-white dark:bg-gray-900 border-b border-gray-200 dark:border-gray-800 shadow-lg"
        >
            <div class="px-4 py-3 space-y-1">
                <!-- Search (mobile) -->
                <div class="mb-4">
                    <div class="relative">
                        <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                            <svg class="h-5 w-5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
                            </svg>
                        </div>
                        <input
                            type="search"
                            placeholder="Search..."
                            class="pl-10 pr-4 py-2 w-full border border-gray-300 dark:border-gray-600 rounded-lg bg-white dark:bg-gray-800 text-gray-900 dark:text-white placeholder-gray-500 dark:placeholder-gray-400 focus:ring-2 focus:ring-indigo-500 focus:border-transparent"
                        />
                    </div>
                </div>

                <!-- Mobile navigation links -->
                <a
                    v-for="item in navigation"
                    :key="item.name"
                    :href="item.href"
                    :class="[
                        item.current
                        ? 'bg-gradient-to-r from-indigo-500 to-blue-500 text-white'
                        : 'text-gray-700 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-800',
                        'block px-4 py-3 rounded-lg text-base font-medium transition-colors'
                    ]"
                    @click="mobileMenuOpen = false"
                >
                    {{ item.name }}
                </a>

                <!-- Mobile auth buttons -->
                <div class="pt-4 border-t border-gray-200 dark:border-gray-800 space-y-3">
                    <Link
                        :href="route('login')"
                        class="block text-center text-gray-700 dark:text-gray-300 hover:text-indigo-600 dark:hover:text-indigo-400 px-4 py-3 text-base font-medium hover:bg-gray-100 dark:hover:bg-gray-800 rounded-lg transition-colors"
                        @click="mobileMenuOpen = false"
                    >
                        Log in
                    </Link>
                    <Link
                        :href="route('register')"
                        class="block text-center bg-gradient-to-r from-indigo-600 to-blue-600 hover:from-indigo-700 hover:to-blue-700 text-white px-4 py-3 rounded-lg text-base font-medium shadow-lg transition-colors"
                        @click="mobileMenuOpen = false"
                    >
                        Get Started
                    </Link>
                </div>
            </div>
        </div>
    </nav>
</template>

<style scoped>
/* Smooth transitions for mobile menu */
.mobile-menu-enter-active,
.mobile-menu-leave-active {
    transition: all 0.3s ease;
}

.mobile-menu-enter-from,
.mobile-menu-leave-to {
    opacity: 0;
    transform: translateY(-10px);
}
</style>
