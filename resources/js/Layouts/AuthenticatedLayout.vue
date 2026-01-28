<script setup>
import { ref, computed, onMounted, watch } from 'vue'
import { Link } from '@inertiajs/vue3'
import ApplicationLogo from '@/Components/ApplicationLogo.vue'
import Dropdown from '@/Components/Dropdown.vue'
import DropdownLink from '@/Components/DropdownLink.vue'
import NavLink from '@/Components/NavLink.vue'
import ResponsiveNavLink from '@/Components/ResponsiveNavLink.vue'

// Theme management
const theme = ref(localStorage.getItem('theme') || 'light')
const sidebarOpen = ref(false)
const userDropdownOpen = ref(false)

const toggleTheme = () => {
    theme.value = theme.value === 'light' ? 'dark' : 'light'
    localStorage.setItem('theme', theme.value)
}

const toggleSidebar = () => {
    sidebarOpen.value = !sidebarOpen.value
}

// Apply theme to document
onMounted(() => {
    applyTheme()
})

watch(theme, () => {
    applyTheme()
})

const applyTheme = () => {
    if (theme.value === 'dark') {
        document.documentElement.classList.add('dark')
    } else {
        document.documentElement.classList.remove('dark')
    }
}

// Navigation items
const navigation = [
    { name: 'Dashboard', href: route('dashboard'), icon: 'M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6' },
    { name: 'Users', href: '#', icon: 'M12 4.354a4 4 0 110 5.292M15 21H3v-1a6 6 0 0112 0v1zm0 0h6v-1a6 6 0 00-9-5.197m13.5 3.75a1.5 1.5 0 01-3 0m3 0a1.5 1.5 0 00-3 0m3 0h6m-9 0H3m0 0a1.5 1.5 0 013 0m0 0a1.5 1.5 0 003 0m0 0h6' },
    { name: 'Products', href: '#', icon: 'M20 7l-8-4-8 4m16 0l-8 4m8-4v10l-8 4m0-10L4 7m8 4v10M4 7v10l8 4' },
    { name: 'Orders', href: '#', icon: 'M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-3 7h3m-3 4h3m-6-4h.01M9 16h.01' },
    { name: 'Analytics', href: '#', icon: 'M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z' },
    { name: 'Settings', href: '#', icon: 'M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z' },
]

const secondaryNavigation = [
    { name: 'Help Center', href: '#', icon: 'M18.364 5.636l-3.536 3.536m0 5.656l3.536 3.536M9.172 9.172L5.636 5.636m3.536 9.192l-3.536 3.536M21 12a9 9 0 11-18 0 9 9 0 0118 0zm-5 0a4 4 0 11-8 0 4 4 0 018 0z' },
    { name: 'Documentation', href: '#', icon: 'M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253' },
]
</script>

<template>
    <div class="min-h-screen bg-gray-50 dark:bg-gray-900 transition-colors duration-200">
        <!-- Mobile sidebar overlay -->
        <div
            v-if="sidebarOpen"
            class="fixed inset-0 z-40 flex md:hidden"
            @click="toggleSidebar"
        >
            <div class="fixed inset-0 bg-gray-600 dark:bg-gray-900 opacity-75"></div>
        </div>

        <!-- Sidebar for desktop -->
        <div class="hidden md:flex md:w-64 md:flex-col md:fixed md:inset-y-0">
            <div class="flex flex-col flex-grow border-r border-gray-200 dark:border-gray-700 pt-5 bg-white dark:bg-gray-800 overflow-y-auto">
                <!-- Logo -->
                <div class="flex items-center flex-shrink-0 px-4">
                    <Link :href="route('dashboard')">
                        <ApplicationLogo class="h-8 w-auto" />
                    </Link>
                    <span class="ml-2 text-xl font-bold text-gray-900 dark:text-white">GMDBR</span>
                </div>

                <!-- Navigation -->
                <div class="mt-8 flex-grow flex flex-col">
                    <nav class="flex-1 px-2 pb-4 space-y-1">
                        <div v-for="item in navigation" :key="item.name">
                            <NavLink
                                :href="item.href"
                                :active="route().current(item.href)"
                                class="group flex items-center px-2 py-2 text-sm font-medium rounded-md"
                            >
                                <svg
                                    class="mr-3 h-5 w-5 flex-shrink-0"
                                    fill="none"
                                    stroke="currentColor"
                                    viewBox="0 0 24 24"
                                >
                                    <path
                                        stroke-linecap="round"
                                        stroke-linejoin="round"
                                        :stroke-width="route().current(item.href) ? '2.5' : '2'"
                                        :d="item.icon"
                                    />
                                </svg>
                                {{ item.name }}
                            </NavLink>
                        </div>
                    </nav>

                    <!-- Secondary navigation -->
                    <div class="px-2 pb-4">
                        <h3 class="px-3 text-xs font-semibold text-gray-500 dark:text-gray-400 uppercase tracking-wider">
                            Support
                        </h3>
                        <div class="mt-1 space-y-1">
                            <a
                                v-for="item in secondaryNavigation"
                                :key="item.name"
                                :href="item.href"
                                class="group flex items-center px-2 py-2 text-sm font-medium rounded-md text-gray-600 hover:text-gray-900 hover:bg-gray-50 dark:text-gray-300 dark:hover:text-white dark:hover:bg-gray-700"
                            >
                                <svg
                                    class="mr-3 h-5 w-5 flex-shrink-0"
                                    fill="none"
                                    stroke="currentColor"
                                    viewBox="0 0 24 24"
                                >
                                    <path
                                        stroke-linecap="round"
                                        stroke-linejoin="round"
                                        stroke-width="2"
                                        :d="item.icon"
                                    />
                                </svg>
                                {{ item.name }}
                            </a>
                        </div>
                    </div>

                    <!-- User profile section -->
                    <div class="flex-shrink-0 flex border-t border-gray-200 dark:border-gray-700 p-4">
                        <div class="flex items-center">
                            <div class="ml-3">
                                <p class="text-sm font-medium text-gray-700 dark:text-gray-300">
                                    {{ $page.props.auth.user.name }}
                                </p>
                                <p class="text-xs font-medium text-gray-500 dark:text-gray-400">
                                    {{ $page.props.auth.user.email }}
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Main content area -->
        <div class="md:pl-64 flex flex-col flex-1">
            <!-- Top navigation bar -->
            <div class="sticky top-0 z-10 flex-shrink-0 flex h-16 bg-white dark:bg-gray-800 shadow">
                <!-- Mobile menu button -->
                <button
                    type="button"
                    class="px-4 border-r border-gray-200 dark:border-gray-700 text-gray-500 dark:text-gray-400 focus:outline-none focus:ring-2 focus:ring-inset focus:ring-indigo-500 md:hidden"
                    @click="toggleSidebar"
                >
                    <span class="sr-only">Open sidebar</span>
                    <svg
                        class="h-6 w-6"
                        fill="none"
                        stroke="currentColor"
                        viewBox="0 0 24 24"
                    >
                        <path
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            stroke-width="2"
                            d="M4 6h16M4 12h16M4 18h7"
                        />
                    </svg>
                </button>

                <div class="flex-1 px-4 flex justify-between">
                    <div class="flex-1 flex items-center">
                        <!-- Search bar -->
                        <div class="max-w-lg w-full lg:max-w-xs">
                            <label for="search" class="sr-only">Search</label>
                            <div class="relative">
                                <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                                    <svg
                                        class="h-5 w-5 text-gray-400"
                                        fill="none"
                                        stroke="currentColor"
                                        viewBox="0 0 24 24"
                                    >
                                        <path
                                            stroke-linecap="round"
                                            stroke-linejoin="round"
                                            stroke-width="2"
                                            d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
                                        />
                                    </svg>
                                </div>
                                <input
                                    id="search"
                                    name="search"
                                    class="block w-full pl-10 pr-3 py-2 border border-gray-300 dark:border-gray-600 rounded-md leading-5 bg-white dark:bg-gray-700 placeholder-gray-500 dark:placeholder-gray-400 focus:outline-none focus:placeholder-gray-400 focus:ring-1 focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm text-gray-900 dark:text-white"
                                    placeholder="Search..."
                                    type="search"
                                />
                            </div>
                        </div>
                    </div>

                    <div class="flex items-center space-x-4">
                        <!-- Theme toggle -->
                        <button
                            @click="toggleTheme"
                            class="p-2 rounded-lg text-gray-500 hover:text-gray-900 dark:text-gray-400 dark:hover:text-white focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
                        >
                            <span class="sr-only">Toggle theme</span>
                            <svg
                                v-if="theme === 'dark'"
                                class="h-5 w-5"
                                fill="none"
                                stroke="currentColor"
                                viewBox="0 0 24 24"
                            >
                                <path
                                    stroke-linecap="round"
                                    stroke-linejoin="round"
                                    stroke-width="2"
                                    d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z"
                                />
                            </svg>
                            <svg
                                v-else
                                class="h-5 w-5"
                                fill="none"
                                stroke="currentColor"
                                viewBox="0 0 24 24"
                            >
                                <path
                                    stroke-linecap="round"
                                    stroke-linejoin="round"
                                    stroke-width="2"
                                    d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z"
                                />
                            </svg>
                        </button>

                        <!-- Notifications -->
                        <button class="p-2 rounded-lg text-gray-500 hover:text-gray-900 dark:text-gray-400 dark:hover:text-white focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                            <span class="sr-only">View notifications</span>
                            <svg
                                class="h-5 w-5"
                                fill="none"
                                stroke="currentColor"
                                viewBox="0 0 24 24"
                            >
                                <path
                                    stroke-linecap="round"
                                    stroke-linejoin="round"
                                    stroke-width="2"
                                    d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9"
                                />
                            </svg>
                        </button>

                        <!-- User dropdown -->
                        <div class="relative">
                            <Dropdown align="right" width="48">
                                <template #trigger>
                                    <button class="flex items-center text-sm border-2 border-transparent rounded-full focus:outline-none focus:border-gray-300 transition">
                                        <span class="sr-only">Open user menu</span>
                                        <div class="h-8 w-8 rounded-full bg-indigo-100 dark:bg-indigo-900 flex items-center justify-center">
                                            <span class="text-sm font-medium text-indigo-600 dark:text-indigo-300">
                                                {{ $page.props.auth.user.name.charAt(0).toUpperCase() }}
                                            </span>
                                        </div>
                                    </button>
                                </template>

                                <template #content>
                                    <DropdownLink :href="route('profile.edit')">
                                        Profile
                                    </DropdownLink>
                                    <DropdownLink :href="route('logout')" method="post" as="button">
                                        Log Out
                                    </DropdownLink>
                                </template>
                            </Dropdown>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Main content -->
            <main class="flex-1">
                <div class="py-6">
                    <div class="max-w-7xl mx-auto px-4 sm:px-6 md:px-8">
                        <!-- Page header -->
                        <div class="mb-6">
                            <slot name="header" />
                        </div>

                        <!-- Page content -->
                        <div class="bg-white dark:bg-gray-800 shadow rounded-lg">
                            <slot />
                        </div>
                    </div>
                </div>
            </main>
        </div>

        <!-- Mobile sidebar -->
        <div
            class="md:hidden fixed inset-y-0 left-0 z-50 w-64 transform transition-transform duration-300 ease-in-out"
            :class="sidebarOpen ? 'translate-x-0' : '-translate-x-full'"
        >
            <div class="flex flex-col h-full bg-white dark:bg-gray-800 shadow-xl">
                <div class="flex items-center justify-between h-16 px-4 border-b border-gray-200 dark:border-gray-700">
                    <div class="flex items-center">
                        <ApplicationLogo class="h-8 w-auto" />
                        <span class="ml-2 text-xl font-bold text-gray-900 dark:text-white">GMDBR</span>
                    </div>
                    <button
                        @click="toggleSidebar"
                        class="p-2 rounded-md text-gray-500 dark:text-gray-400 hover:text-gray-900 dark:hover:text-white"
                    >
                        <span class="sr-only">Close sidebar</span>
                        <svg
                            class="h-6 w-6"
                            fill="none"
                            stroke="currentColor"
                            viewBox="0 0 24 24"
                        >
                            <path
                                stroke-linecap="round"
                                stroke-linejoin="round"
                                stroke-width="2"
                                d="M6 18L18 6M6 6l12 12"
                            />
                        </svg>
                    </button>
                </div>

                <div class="flex-1 overflow-y-auto">
                    <nav class="px-2 py-4 space-y-1">
                        <div v-for="item in navigation" :key="item.name">
                            <ResponsiveNavLink
                                :href="item.href"
                                :active="route().current(item.href)"
                                class="group flex items-center px-2 py-2 text-base font-medium rounded-md"
                            >
                                <svg
                                    class="mr-4 h-6 w-6 flex-shrink-0"
                                    fill="none"
                                    stroke="currentColor"
                                    viewBox="0 0 24 24"
                                >
                                    <path
                                        stroke-linecap="round"
                                        stroke-linejoin="round"
                                        :stroke-width="route().current(item.href) ? '2.5' : '2'"
                                        :d="item.icon"
                                    />
                                </svg>
                                {{ item.name }}
                            </ResponsiveNavLink>
                        </div>
                    </nav>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
/* Custom scrollbar for sidebar */
::-webkit-scrollbar {
    width: 6px;
}

::-webkit-scrollbar-track {
    background: transparent;
}

::-webkit-scrollbar-thumb {
    background: #d1d5db;
    border-radius: 3px;
}

.dark ::-webkit-scrollbar-thumb {
    background: #4b5563;
}

::-webkit-scrollbar-thumb:hover {
    background: #9ca3af;
}

.dark ::-webkit-scrollbar-thumb:hover {
    background: #6b7280;
}
</style>
