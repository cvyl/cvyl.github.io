---
title: "Friendships"
description: "A dynamic page listing my friends with sections for different categories."
comment: false
---

<!-- Load Vue -->
{% raw %}
<script src="https://unpkg.com/vue@3"></script>

<!-- Vue App -->
<script>
document.addEventListener('DOMContentLoaded', () => {
  const app = Vue.createApp({
    data() {
      return {
        friendsSections: []  // Holds the sections data
      };
    },
    mounted() {
      // Fetch the JSON data
      fetch('./friends.json')  // Adjust the path to your JSON file
        .then(response => response.json())
        .then(data => {
          this.friendsSections = data;
        })
        .catch(error => console.error('Error fetching friends data:', error));
    }
  });

  app.mount('#friends-app');

  // Hexo patch: Destroy app when page switched
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
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); /* Create dynamic grid with min and max widths */
  gap: 20px; /* Space between items */
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
  height: 100%; /* Ensure equal height for all items */
}

.friend-item:hover {
  transform: scale(1.05);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

.friend-item img {
  width: 50px; /* Icon size */
  height: 50px;
  margin-bottom: 10px;
  border-radius: 50%; /* Make the icon circular */
}

.friend-item-title {
  font-size: 1.2rem;
  margin-bottom: 5px;
  color: #333;
}

.friend-item-desc {
  font-size: 0.9rem;
  color: #777;
}

.friend-link {
  color: inherit;
  text-decoration: none;
  display: block; /* Ensure the entire item is clickable */
}

@media (max-width: 1200px) {
  .friends-grid {
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); /* Slightly smaller for medium screens */
  }
}

@media (max-width: 800px) {
  .friends-grid {
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); /* Smaller for small screens */
  }
}

@media (max-width: 600px) {
  .friends-grid {
    grid-template-columns: 1fr; /* Stack items in one column on very small screens */
  }
}
</style>

<!-- Template for Friends -->
<div id="friends-app" class="friends-page">
  <div v-if="friendsSections.length === 0">Loading...</div>
  <div v-for="section in friendsSections" :key="section.title" class="friend-category">
    <h2>{{ section.title }}</h2>
    <div class="friends-grid">
      <div v-for="friend in section.items" :key="friend.title" class="friend-item">
        <a :href="friend.link" target="_blank" class="friend-link">
          <img v-if="friend.icon" :src="friend.icon" alt="Friend Icon" />
          <div class="friend-item-title">{{ friend.title }}</div>
          <div class="friend-item-desc">{{ friend.desc }}</div>
        </a>
      </div>
    </div>
  </div>
</div>

{% endraw %}
