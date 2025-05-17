<template>
  <div ref="scrollContainer" class="scroll-container" @scroll="onScroll">
    <div :style="{ height: totalHeight + 'px', position: 'relative' }">
      <div
        v-for="(item, index) in visibleItems"
        :key="startIndex + index"
        :style="getItemStyle(startIndex + index)"
        :class="['item', cardView ? 'card' : 'list', isFavorite(item) ? 'favorite' : '']"
        @click="$emit('select', item)"
        @mouseover="hovered = item.login.uuid"
        @mouseleave="hovered = null"
      >
        <img :src="item.picture.thumbnail" />
        <div class="info">
          <strong>{{ item.name.first }} {{ item.name.last }}</strong>
          <p v-if="!cardView">{{ item.email }}</p>
        </div>
        <button
          class="like"
          @click.stop="$emit('toggle-favorite', item)"
          :title="isFavorite(item) ? 'Unfavorite' : 'Favorite'"
        >
          
        </button>

        <div v-if="hovered === item.login.uuid && cardView" class="hover-info">
          <p><strong>Email:</strong> {{ item.email }}</p>
          <p><strong>City:</strong> {{ item.location.city }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    items: Array,
    itemHeight: Number,
    visibleCount: Number,
    buffer: Number,
    cardView: Boolean,
    favorites: Array
  },
  data() {
    return {
      startIndex: 0,
      hovered: null
    }
  },
  computed: {
    totalHeight() {
      return this.items.length * this.itemHeight;
    },
    visibleItems() {
      const end = Math.min(this.startIndex + this.visibleCount + this.buffer, this.items.length);
      return this.items.slice(this.startIndex, end);
    }
  },
  methods: {
    onScroll() {
      const scrollTop = this.$refs.scrollContainer.scrollTop;
      this.startIndex = Math.floor(scrollTop / this.itemHeight);
    },
    getItemStyle(index) {
      return {
        position: 'absolute',
        top: index * this.itemHeight + 'px',
        width: '100%',
        padding: this.cardView ? '15px' : '10px'
      }
    },
    isFavorite(user) {
      return this.favorites.includes(user.login.uuid);
    }
  }
}
</script>

<style scoped>
.scroll-container {
  height: 900px;
  overflow-y: auto;
  border: 1px solid #ddd;
  margin: 20px auto;
  width: 90%;
  max-width: 700px;
  background: #f8fff8;
  border-radius: 10px;
}
.item {
  display: flex;
  align-items: center;
  background: white;
  border: 1px solid #d8f5dc;
  margin: 8px;
  border-radius: 12px;
  cursor: pointer;
  transition: box-shadow 0.3s, transform 0.2s;
}
.item:hover {
  box-shadow: 0 2px 12px rgba(0, 80, 50, 0.2);
}
.item img {
  border-radius: 50%;
  margin-right: 12px;
  width: 50px;
}
.item .info {
  flex: 1;
  text-align: left;
}
.card {
  flex-direction: column;
  align-items: flex-start;
}
.card img {
  margin-bottom: 8px;
}
.like {
  background: transparent;
  border: none;
  font-size: 18px;
  cursor: pointer;
  color: #ccc;
  margin-left: auto;
}
.favorite .like {
  color: #e74c3c;
}
.hover-info {
  margin-top: 6px;
  font-size: 12px;
  color: #22543d;
  background: #f0fff4;
  padding: 6px;
  border-radius: 8px;
}
</style>
