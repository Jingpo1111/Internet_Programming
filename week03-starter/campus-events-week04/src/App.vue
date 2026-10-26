<script setup lang="ts">
import { computed, ref, watch } from "vue";
import EventList from "./components/EventList.vue";
import { initialEvents } from "./data/events";
import type { CampusEvent } from "./types";

// A reactive copy of the hard-coded events
const events = ref<CampusEvent[]>(initialEvents.map(e => ({ ...e })));

function loadBookings(): number[] {
  try {
    const raw = localStorage.getItem(STORAGE_KEY);
    if (!raw) return [];
    const parsed: unknown = JSON.parse(raw);
    return Array.isArray(parsed)
      ? parsed.filter((x): x is number => typeof x === "number")
      : [];
  } catch {
    return [];
  }
}

// TODO 1: a ref `myBookings` holding the ids of the events I booked (one entry per seat)
const myBookings = ref<number[]>(loadBookings());
for (const id of myBookings.value) {
  const event = events.value.find(e => e.id === id);
  if (event && seatsLeft(event) > 0) event.booked++;
}
// TODO 2: a computed `bookingCount`
const bookingCount = computed(() => myBookings.value.length);

// TODO 3: onBook(id): find the event; if it still has a seat,
//         increase its `booked` and add the id to myBookings

function onBook(id: number) {
  const event = events.value.find(e => e.id === id);
  if (!event || seatsLeft(event) <= 0) return;
  event.booked++;
  myBookings.value.push(id);
}
 

// TODO 4 (bonus): keep my bookings after a page reload
//   - watch myBookings and save it to localStorage (JSON.stringify)
//   - read it back when the app starts, and add those seats to `booked`
watch(
  myBookings,
  (ids) => localStorage.setItem(STORAGE_KEY, JSON.stringify(ids)),
  { deep: true }
);
</script>

<template>
  <main>
    <header>
      <h1>Campus Events</h1>
      <!-- TODO 5: show the real count ("1 seat" / "N seats") -->
      <p class="mine">
        You have booked {{ bookingCount }} {{ bookingCount === 1 ? "seat" : "seats" }}.
      </p>
    </header>
    <EventList :events="events" @book="onBook" />
  </main>
</template>

<style scoped>
header { margin-bottom: 16px; }
.mine { color: #6b7280; margin: 0; }
</style>
