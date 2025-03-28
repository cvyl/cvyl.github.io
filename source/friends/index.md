---
title: "Friendships"
description: "A dynamic page listing my friends with different categories."
comment: false
---

<!-- TODO:
  Download all profile pictures into /gallery  
-->

{% raw %}
<script src="https://cdn.jsdelivr.net/npm/vue@3"></script>

<script>
document.addEventListener('DOMContentLoaded', () => {
  const app = Vue.createApp({
    data() {
      return {
        friendsSections: [],
        loading: true
      };
    },
    methods: {
      async checkImage(url) {
        try {
          const response = await fetch(url, { method: 'HEAD' });
          if (!response.ok) throw new Error('Image not found');
          return url;
        } catch {
          return 'gallery/noimage.png';
        }
      },
      async processFriendsData(data) {
        this.friendsSections = data.map(section => ({
          ...section,
          items: section.items.map(friend => ({
            ...friend,
            imageLoaded: false,
            icon: friend.icon || 'gallery/noimage.png'
          }))
        }));
        this.loading = false;
        
        for (let section of this.friendsSections) {
          for (let friend of section.items) {
            const validIcon = await this.checkImage(friend.icon);
            friend.icon = validIcon;
            friend.imageLoaded = true;
          }
        }
      }
    },
    mounted() {
      fetch('./friends.json')
        .then(response => response.json())
        .then(data => this.processFriendsData(data))
        .catch(error => console.error('Error fetching friends data:', error));
    }
  });

  app.mount('#friends-app');

  const interval = setInterval(() => {
      if (!document.getElementById('friends-app')) 
      {
          app.unmount();
          clearInterval(interval);
      }
  }, 1000);
});
</script>

<!-- Styles for Friends Page -->
<style>
.friends-page {
  max-width: 1600px;
  margin: auto;
  padding: 20px;
}

.friend-category {
  margin-bottom: 40px;
}

.friends-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 20px;
}

.friend-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px 20px 10px 20px;
  border-radius: 12px;
  background-color: #f9f9f9;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  text-align: center;
  height: 100%;
}

.friend-item:hover {
  transform: scale(1.05);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

.friend-item img {
  width: 50px;
  height: 50px;
  margin-bottom: 10px;
  border-radius: 50%;
  transition: opacity 0.3s ease;
}

.friend-item .skeleton {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
  background-size: 200% 100%;
  animation: loading 1.5s infinite;
}

@keyframes loading {
  0% { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}

.friend-item-title {
  font-size: 1.2rem;
  margin-bottom: 5px;
  color: #333;
}

.friend-item-desc {
  font-size: 0.9rem;
  color: #777;
  white-space: pre-line;
}

.friend-link {
  color: inherit;
  text-decoration: none;
  display: block;
}

@media (max-width: 1200px) {
  .friends-grid {
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  }
}

@media (max-width: 800px) {
  .friends-grid {
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  }
}

@media (max-width: 600px) {
  .friends-grid {
    grid-template-columns: 1fr;
  }
}
</style>

<div id="friends-app" class="friends-page">
  <div v-if="loading">Loading...</div>
  <div v-else v-for="section in friendsSections" :key="section.title" class="friend-category">
    <h2>{{ section.title }}</h2>
    <div class="friends-grid">
      <div v-for="friend in section.items" :key="friend.title" class="friend-item">
        <a :href="friend.link" target="_blank" class="friend-link">
          <div v-if="!friend.imageLoaded" class="skeleton"></div>
          <img v-else :src="friend.icon" alt="Friend Icon" />
          <div class="friend-item-title">{{ friend.title }}</div>
          <div class="friend-item-desc">{{ friend.desc }}</div>
        </a>
      </div>
    </div>
  </div>
</div>

{% endraw %}
