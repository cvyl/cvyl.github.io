---
title: Navigation Page
description: A dynamic navigation page for useful links.
comment: false
---

<!-- Load Vue and Navigation Data -->
{% raw %}
<script src="https://cdn.jsdelivr.net/npm/vue@3"></script>

<!-- Vue App -->
<script>
document.addEventListener('DOMContentLoaded', () => {
  const app = Vue.createApp({
    data() {
      return {
        navData: []  // This will hold the data fetched from the JSON file
      };
    },
    mounted() {
      // Fetch the JSON data
      fetch('./data.json')  // Adjust the path to where your nav-data.json file is stored
        .then(response => response.json())
        .then(data => {
          this.navData = data;  // Assign the fetched data to navData
        })
        .catch(error => console.error('Error fetching navigation data:', error));
    }
  });

  app.mount('#nav-app');

  // Hexo patch: Destroy app when page switched
  const interval = setInterval(() => {
      if (!document.getElementById('nav-app')) 
      {
          app.unmount();
          clearInterval(interval);
      }
  }, 1000);
});
</script>

<!-- Styles for Navigation Page -->
<style>
.nav-page {
  max-width: 1600px;
  margin: auto;
  padding: 20px;
}

.nav-category {
  margin-bottom: 40px;
}

.nav-category h2 {
  font-size: 1.8rem;
  margin-bottom: 20px;
  color: #333; /* Set a custom title color */
}

.nav-items {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* 3 items per row */
  gap: 20px; /* Space between items */
}

.nav-item {
  height: auto; /* Ensure dynamic height */
  width: auto;
  padding: 10px;
  border-radius: 8px;
  text-align: left;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  background-color: #f9f9f9; /* Light background */
}

.nav-item:hover {
  transform: scale(1.05);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

.nav-item img {
  width: 30px;
  height: 30px;
  margin-right: 10px;
  float: left;
}

.nav-item-title {
  font-size: clamp(1rem, 2.5vw, 1.2rem); /* Dynamically adjust the font size */
  margin-bottom: 10px;
  color: #333;
  white-space: normal; /* Allow text to wrap */
  overflow: hidden; /* Hide overflow if any */
  display: -webkit-box;
  -webkit-line-clamp: 2; /* Limit to 2 lines */
  -webkit-box-orient: vertical;
  
}

.nav-item-desc {
  font-size: 0.9rem;
  color: #777;
}

.nav-link {
  color: inherit; /* This ensures the link inherits the color from the parent container */
  text-decoration: none; /* Removes the underline from the link */
  display: block; /* Ensure entire link is clickable */
}

.nav-link:hover {
  color: #555; /* Optional: change the color on hover to a slightly darker shade */
}

/* Ensure responsiveness for smaller screens */
@media (max-width: 1200px) {
  .nav-items {
    grid-template-columns: repeat(2, 1fr); /* Adjust to 2 items per row on medium screens */
  }
}

@media (max-width: 800px) {
  .nav-items {
    grid-template-columns: 1fr; /* Stack items in one column on small screens */
  }
}

</style>

<!-- Template for Navigation Links -->
<div id="nav-app" class="nav-page">
  <div v-if="navData.length === 0">Loading...</div>
  <div v-for="category in navData" :key="category.title" class="nav-category">
    <h2>{{ category.title }}</h2>
    <div class="nav-items">
      <div v-for="item in category.items" :key="item.title" class="nav-item">
        <a :href="item.link" target="_blank" class="nav-link">
          <img v-if="item.icon" :src="item.icon" alt="icon" />
          <div class="nav-item-title">{{ item.title }}</div>
          <div class="nav-item-desc">{{ item.desc }}</div>
        </a>
      </div>
    </div>
  </div>
</div>

{% endraw %}
