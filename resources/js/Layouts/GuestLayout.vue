<script setup>
import Navbar from '@/Components/Navbar.vue'
import Footer from '@/Components/Footer.vue'
</script>

<template>
    <div class="min-h-screen flex flex-col bg-gray-50 dark:bg-gray-900 transition-colors duration-200">
        <Navbar />
        <main class="flex-grow pt-20">
            <slot />
        </main>
        <Footer />
    </div>
</template>
