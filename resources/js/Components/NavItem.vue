<template>
  <Link
    :href="item.href"
    :class="[
      'group flex items-center px-3 py-2 text-sm font-medium rounded-md transition-colors',
      active ? 'bg-gray-800 text-white' : 'text-gray-300 hover:bg-gray-800 hover:text-white'
    ]"
    :title="collapsed ? item.name : ''"
  >
    <!-- Icon -->
    <svg
      :class="[
        active ? 'text-white' : 'text-gray-400 group-hover:text-white',
        collapsed ? 'mx-auto h-6 w-6' : 'mr-3 h-5 w-5 flex-shrink-0'
      ]"
      fill="none"
      stroke="currentColor"
      viewBox="0 0 24 24"
    >
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" :d="item.icon" />
    </svg>

    <!-- Label (hidden when collapsed) -->
    <span v-show="!collapsed" class="truncate">{{ item.name }}</span>

    <!-- Badge (optional) -->
    <span
      v-if="item.badge && !collapsed"
      :class="[
        'ml-auto inline-block py-0.5 px-2 text-xs rounded-full',
        active ? 'bg-indigo-600 text-white' : 'bg-gray-700 text-gray-300'
      ]"
    >
      {{ item.badge }}
    </span>
  </Link>
</template>

<script setup>
import { Link } from '@inertiajs/vue3'

defineProps({
  item: Object,
  collapsed: Boolean,
  active: Boolean
})
</script>
