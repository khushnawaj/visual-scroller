# Virtual Scrolling User List App

A Vue 3 application that efficiently renders a large list of users (1000+) using virtual scrolling, featuring a wellness-inspired design with white and forest green colors. The app includes user details modals, favorites, list/card view toggle, and smooth user experience enhancements.

---

## Features

- **Virtual Scrolling:** Only visible items are rendered to improve performance with large datasets.
- **Data Source:** Fetches user data from [Random User API](https://randomuser.me).
- **Wellness Theme:** White and forest green color scheme conveying a calm and fresh look.
- **Favorites:** Users can mark/unmark favorites, persisted in localStorage.
- **View Toggle:** Switch between list and card layouts.
- **User Modal:** Click a user to view detailed information in a modal.
- **Hover Popups:** Quick user info on hover.

---

## How It Works

### Virtual Scrolling Concept

Rendering thousands of DOM nodes slows performance drastically. Virtual scrolling solves this by rendering **only the items visible in the viewport plus a small buffer**. The container mimics the full height of the list, while items are absolutely positioned inside it according to scroll position.

This drastically reduces DOM nodes and repaint workload, making scrolling smooth even with huge datasets.

### Data Flow

1. **App.vue** fetches 1000 user records from the Random User API and stores them in `allUsers`.
2. Data is passed as props to the **VirtualList.vue** component.
3. **VirtualList.vue** listens to scroll events, calculates visible item indices, and renders only those items.
4. Clicking an item selects it, and **UserModal.vue** displays detailed info.
5. Favorites are managed in App.vue and passed down as props, with persistence in `localStorage`.

---

## Role of the DOM

- The DOM renders only a small subset of list items based on scroll position.
- A fixed-height container provides the illusion of the full scroll area.
- Items inside are positioned absolutely to avoid layout recalculations.
- This technique reduces browser rendering load and keeps the interface responsive.

---

## Vue Component Details

### `App.vue`

- **data():** Holds user data (`allUsers`), UI state (`selectedUser`, `favorites`, `cardView`).
- **mounted():** Fetches user data from the API.
- **methods:** 
  - `selectUser(user)` opens the modal.
  - `toggleFavorite(user)` adds/removes favorites.
  - `toggleView()` switches list/card layout.
- **computed:** Manages filtered or derived data if needed.
- Passes props and events to child components.

### `VirtualList.vue`

- **props:** `items`, `itemHeight`, `visibleCount`, `buffer`, `cardView`, `favorites`.
- **data():** Manages `startIndex` for the top visible item and `hovered` user.
- **computed:** 
  - `totalHeight`: total height of all items combined.
  - `visibleItems`: slice of items currently visible plus buffer.
- **methods:**
  - `onScroll(event)`: updates `startIndex` based on scroll position.
  - `getItemStyle(index)`: calculates absolute position of each rendered item.
  - `isFavorite(user)`: checks if user is favorited.
- Uses event listeners on the scroll container to recalc visible range on scroll.

### `UserModal.vue`

- **props:** Receives the selected `user` object.
- Emits a `close` event when clicking outside the modal or pressing the close button.
- Displays detailed user info: photo, name, email, phone, location, registered date.
- Styled with a calming white and forest green theme.

---

## Lifecycle Hooks

- `mounted()`: Used in `App.vue` to trigger the initial fetch of user data once the component is mounted.

---

## Summary

This project demonstrates how advanced DOM manipulation combined with Vue reactive data and computed properties can build a performant and user-friendly large dataset UI. The key is minimizing DOM nodes via virtual scrolling, efficiently updating only what’s needed, and layering polished UI components like modals and favorites to enrich user interaction — all within Vue 3’s modern Composition API style.

---

## Running the Project

1. Clone the repo.
2. Run `npm install`.
3. Run `npm run dev` (for Vite) or `npm run serve` (for Vue CLI).
4. Open `http://localhost:3000` or the local dev server URL.
5. Scroll through the user list, toggle views, favorite users, and open modals.

---

## Credits

- User data provided by [Random User API](https://randomuser.me).- UI inspired by wellness color schemes and modern minimal design principles.

---

Feel free to reach out for questions or feature requests!
