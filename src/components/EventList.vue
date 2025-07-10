<template>
  <template v-if="error">
    <sectionCard>
          <div class="space-y-4 items-center flex flex-col">
            <div class="text-red-500"> Could not fetch events at this moment. </div>
            <RoundButton @click="fetchEvents"> Retry It</RoundButton>
          </div>
    </sectionCard>
  </template>
  <template v-else>
    <section class="grid grid-cols-1 lg:grid-cols-2 gap-10">
      <template v-if="!eventsLoading">
         <template v-if="events.length">
           <EventCard
            v-for="event in events"
            :key="event.id"
            :title="event.title"
            :when="event.date"
            :description="event.description"
            @register="$emit('register', event)"
            />
         </template>
         <template v-else>
           <!-- <div class="col-span-2 text-center"> No events</div> -->
           <LoadingEvents v-for="i in 4" :key="i" />
         </template>
      </template>
      <template v-else>
        <LoadingEvents v-for="i in 4" :key="i" />
      </template>
    </section>
  </template>
</template>

<script setup>
  import { ref, onMounted } from 'vue'
  import EventCard from '@/components/EventCard.vue' ;
  import LoadingEvents from '@/components/LoadingEvents.vue';
  import SectionCard from '@/components/SectionCard.vue';
  import RoundButton from '@/components/RoundButton.vue';

  const events = ref([]);
  const eventsLoading = ref(false);
  const error = ref(null);

  const fetchEvents = async () => {
  eventsLoading.value = true
  error.value = null;
  try {
    const response = await fetch('http://localhost:3002/events')
    events.value = await response.json()
  } catch(e){
    error.value = e;

  }
  finally {
    eventsLoading.value = false
  }
}

onMounted(() => {
  fetchEvents();
})

</script>
