<template>
  <div class="app-container">
    <h2>User Scroller</h2>
    <div class="controls">
      <input v-model="search" placeholder="Search users..." />
      <select v-model="sortBy">
        <option value="">Sort By</option>
        <option value="name">Name</option>
        <option value="email">Email</option>
        <option value="city">City</option>
      </select>
      <button @click="toggleView">{{ isCardView ? 'List View' : 'Card View' }}</button>
    </div>

    <p class="user-count">Found {{ filteredUsers.length }} users</p>

    <VirtualList v-if="filteredUsers && filteredUsers.length" :items="filteredUsers" :itemHeight="isCardView ? 150 : 90"
      :visibleCount="10" :buffer="5" :cardView="isCardView" :favorites="favorites" @toggle-favorite="toggleFavorite"
      @select="selectedUser = $event" />

    <button class="scroll-top" @click="scrollToTop">↑ Top</button>

    <div v-if="selectedUser" class="modal" @click.self="selectedUser = null">
      <div class="modal-content">
        <h3>{{ selectedUser.name.first }} {{ selectedUser.name.last }}</h3>
        <img :src="selectedUser.picture.large" />
        <p><strong>Email:</strong> {{ selectedUser.email }}</p>
        <p><strong>Phone:</strong> {{ selectedUser.phone }}</p>
        <p><strong>City:</strong> {{ selectedUser.location.city }}</p>
        <p><strong>Date :</strong> {{ selectedUser.location.city }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import VirtualList from './components/VirtualList.vue';

export default {
  components: { VirtualList },
  data() {
    return {
      users: [],
      search: '',
      sortBy: '',
      selectedUser: null,
      isCardView: false,
      favorites: JSON.parse(localStorage.getItem('favorites') || '[]')
    }
  },
  computed: {
    filteredUsers() {
      if (!Array.isArray(this.users)) return [];
      let users = this.users;
      if (this.search) {
        const term = this.search.toLowerCase();
        users = users.filter(u =>
          u.name.first.toLowerCase().includes(term) ||
          u.name.last.toLowerCase().includes(term) ||
          u.email.toLowerCase().includes(term) ||
          u.location.city.toLowerCase().includes(term)
        );
      }
      if (this.sortBy) {
        users = [...users].sort((a, b) => {
          if (this.sortBy === 'name') return a.name.first.localeCompare(b.name.first);
          if (this.sortBy === 'email') return a.email.localeCompare(b.email);
          if (this.sortBy === 'city') return a.location.city.localeCompare(b.location.city);
          return 0;
        });
      }
      return users;
    }
  },
  methods: {
    toggleView() {
      this.isCardView = !this.isCardView;
    },
    toggleFavorite(user) {
      const id = user.login.uuid;
      if (this.favorites.includes(id)) {
        this.favorites = this.favorites.filter(f => f !== id);
      } else {
        this.favorites.push(id);
      }
      localStorage.setItem('favorites', JSON.stringify(this.favorites));
    },
    scrollToTop() {
      document.querySelector('.scroll-container')?.scrollTo({ top: 0, behavior: 'smooth' });
    }
  },
  async mounted() {
    const res = await axios.get('https://randomuser.me/api/?results=1000');
    this.users = res.data.results;
  }
}
</script>

<style scoped>
.app-container {
  text-align: center;
  padding: 20px;
  background-color: #f5fff5;
  font-family: 'Segoe UI', sans-serif;
}

h2 {
  color: #22543d;
  margin-bottom: 10px;
}

.controls {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-bottom: 10px;
}

.controls input,
.controls select,
.controls button {
  padding: 8px;
  border-radius: 8px;
  border: 1px solid #ccc;
}

.user-count {
  color: #22543d;
  margin-bottom: 5px;
}

.scroll-top {
  margin-top: 10px;
  padding: 6px 12px;
  background: #22543d;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
}

.scroll-top:hover {
  background-color: #1b3a2e;
}

.modal {
  position: fixed;
  inset: 0;
  background: rgba(0, 60, 30, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 12px;
  text-align: center;
}

.modal-content img {
  width: 100px;
  border-radius: 50%;
  margin: 10px 0;
}
</style>
