<template>
  <button
    
    :class="[
      'text-sm font-medium  rounded-full px-3 py-1 hover:bg-blue-400 cursor-pointer',
      variants[variant] ?? variants.default,
    ]"
    @click="$emit('click')"
  >
    {{ label }}
    <!-- <slot></slot> -->
  </button>
</template>

<script setup>
defineEmits(['click'])

const variants = {
  default: 'border bg-gray-100 hover: bg-blue-100 ',
  danger: 'bg-red-100 enabled:hover: bg-red-100 text-red-900 ',
}

defineProps({
  variant: {
    type: String,
    default: 'default',
  },
  label: {
    type: String,
    default: 'Click me',
  },
})
</script>
