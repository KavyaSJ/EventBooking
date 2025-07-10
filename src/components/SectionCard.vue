<template>
 <RoundCard>
  <div class="border border-gray-400 bg-white-100 rounded-md">
      <h3 v-if="$slots.header" class="p-4 border-b" font-medium>
        <slot name="header"></slot>
      </h3>
      <div  v-if="$slots.default" class="p-4 border-b">
         <slot></slot>
      </div>
      <div v-if="$slots.footer" class="p-4">
        <slot name="footer"></slot>
     </div>
  </div>

 </RoundCard>
</template>

<script setup>
import RoundCard from "@/components/RoundCard.vue";

</script>
