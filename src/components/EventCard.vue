<template>
  <SectionCard>
      <template #header>
          {{title}}
      </template>

       {{when}}

      <template #footer>
            {{description}}
          <RoundButton class="mt-4" label="Rigister" @click="$emit('register')">

          </RoundButton>
      </template>
  </SectionCard>
</template>

<script setup>

import SectionCard from "@/components/SectionCard.vue";
import RoundButton from "@/components/RoundButton.vue";

defineProps({
  title: String,
  when: String,
  description: String
});
defineEmits(['register']);

</script>

