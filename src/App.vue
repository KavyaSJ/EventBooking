<template>
  <main class="container mx-auto my-8 space-y-8">
    <h1 class="text-3xl font-bold">Event Booking</h1>
    <h2 class="text-xl font-semibold">All Events</h2>

     <EventList @register="handleRegistration($event)" />
    <h2>Your bookings</h2>

    <section class="grid gap-10">
      <!-- <BookingCard v-for="i in 3" :key="i" /> -->

      <template v-if="!bookingsLoading">
        <BookingCard v-for="booking in bookings"
        :key="booking.id" :title="booking.eventTitle" :status="booking.status"
         @cancelled="cancelBooking(booking.id)"/>
      </template>
      <template v-else>
         <LoadingBookingItem v-for="i in 4" :key="i"></LoadingBookingItem>
      </template>

    </section>
  </main>
</template>

<script setup>
import { ref, onMounted } from 'vue'

import BookingCard from '@/components/BookingCard.vue';
import LoadingBookingItem from '@/components/LoadingBookingItem.vue';
import EventList from '@/components/EventList.vue';



const bookings = ref([]);
const bookingsLoading = ref(false);



const fetchBookings = async () => {
  bookingsLoading.value = true;
  try{
    const response = await fetch('http://localhost:3002/bookings');
    bookings.value = await response.json();
  }finally{
    bookingsLoading.value = false;
  }
};

const findBookingById = (id) => bookings.value.findIndex((b) => b.id === id);

const handleRegistration = async (event) => {
  if(bookings.value.some((bookings) => bookings.eventId === event.id && bookings.UserId === 1)){
    alert('You are already register for this event.');
    return;
  }
  console.log(event);
  const newBooking = {
    id: Date.now().toString(),
    UserId: 1,
    eventId: event.id,
    eventTitle: event.title,
    status: 'pending',
  }

  bookings.value.push(newBooking);

  try{
    const response = await fetch('http://localhost:3002/bookings', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      ...newBooking,
      status: 'confirmed'
    })
  });

  if(response.ok){
    const index = findBookingById(newBooking.id);
    bookings.value[index] = await response.json();
  }
  else{
    throw new Error('Failed to confirm booking');
  }

  }catch(e){
    console.error('failed to register for the event:', e);
    bookings.value = bookings.value.filter((b) => b.id != newBooking.id);

  }
}

const cancelBooking = async (bookingId) => {
  const index = findBookingById(bookingId);
  const originalBooking = bookings.value[index];
  bookings.value.splice(index, 1);

  try{
    const response = await fetch(`http://localhost:3002/bookings/${bookingId}`, {
      method: 'DELETE'
    });
    if(!response.ok){
      throw new Error('Booking could not be cancelled.');
    }

  }catch(e){
     console.error('Failed to cancel booking:', e);
     bookings.value.splice(index,0, originalBooking);
  }
};


onMounted(() => {

  fetchBookings();
});
</script>
