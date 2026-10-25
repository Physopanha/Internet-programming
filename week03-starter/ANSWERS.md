# Week 3 Answers

**1. In EventList, what goes wrong if you use the array index as key and the list gets filtered while a card has local state (for example an open details panel)?**
If you use the array index as a `:key`, Vue ties the DOM element and local component state to that position index rather than the specific event's identity. When the list is filtered, a different event shifts into that index position, and Vue incorrectly reuses the old component state (like the open panel) for the new event data. This causes the visual UI state to mismatch the actual underlying data.

**2. Why is visible a computed and not a function called from the template? What is the difference when you type in the search box?**
A `computed` property caches its result and only recalculates when its reactive dependencies (like the `search` text or `category`) actually change. A function called directly from the template would re-run on every single component re-render, which is inefficient. When typing in the search box, `computed` ensures the filtered list is evaluated exactly once per keystroke and efficiently shares that result with both the counter text and the `v-for` loop.

**3. EventCard emits book instead of changing event.booked. Give two reasons.**
First, Vue enforces a one-way data flow where props passed down to child components are read-only, and Vue will throw a warning if you try to mutate them directly. Second, incrementing `event.booked` directly inside `EventCard` would prevent the parent `App.vue` from simultaneously updating the `myBookings` list or triggering the `localStorage` save. Emitting an event ensures the component that owns the state (`App.vue`) handles all related mutations safely.

**4. You write `const { booked } = reactive(event); booked++;` The page does not change. Why?**
Destructuring a primitive value (like the `booked` number) from a `reactive` object extracts only the plain JavaScript value, completely breaking the reactive connection to Vue. When you run `booked++`, you are only updating a disconnected, local variable in memory. Because the reactivity proxy is lost, Vue does not detect this change and the page does not re-render.

**5. Where does myBookings live, and why not in EventCard or EventList?**
The `myBookings` state lives in the top-level `App.vue` component. It cannot live in `EventCard` because each card would isolatedly track its own bookings, and it cannot live in `EventList` because the header (which needs to display the total booking count) would not have access to it. Placing it in `App.vue` makes it accessible to both the header and the event handlers.