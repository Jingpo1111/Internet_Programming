# Week 4 practice: Vue components and reactivity

# Question

## 1. In EventList, what goes wrong if you use the array index as :key and the list gets filtered while a card has local state (for example an open details panel)?

## - Index as key: After filtering, the positions shift, so Vue reuses the wrong card instance. Local state like an open panel then shows on a different event. Use e.id so each card keeps its own state.

## 2. Why is visible a computed and not a function called from the template? What is the difference when you type in the search box?

## - Computed vs function: A computed is cached and recalculates only when search, category or events change. A function in the template runs on every re-render. Typing in the search box changes search, so the computed recalculates once per keystroke.

## 3. EventCard emits book instead of changing event.booked. Give two reasons.

## -Why emit book: Props are read-only, so a child must not change them. App owns the data and the booking rules, and the card only reports that the user wants to book.

## 4. You write const { booked } = reactive(event); booked++. The page does not change.Why?

## - Destructuring: const { booked } = reactive(event) copies the number into a plain variable that Vue cannot track. booked++ changes only that copy, so the page stays the same. Use event.booked++ instead.

## 5. Where does myBookings live, and why not in EventCard or EventList?

## - myBookings: It lives in App.vue because App owns the events and the header needs the count. EventCard and EventList can’t share it, and the state would be lost if either was re-created.
