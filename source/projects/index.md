---
title: Projects Page
description: A list of some of the projects I have worked on.
comment: false
---

{% raw %}
<script src="https://cdn.jsdelivr.net/npm/vue@3"></script>

<script>
document.addEventListener('DOMContentLoaded', () => {
  const app = Vue.createApp({
    data() {
      return {
        projects: []
      };
    },
    mounted() {
      fetch('./projects.json')
        .then(res => res.json())
        .then(data => {
          this.projects = data;
        })
        .catch(err => console.error('Failed to load projects:', err));
    }
  });

  app.mount('#projects-app');

  const interval = setInterval(() => {
    if (!document.getElementById('projects-app')) {
      app.unmount();
      clearInterval(interval);
    }
  }, 1000);
});
</script>

<style>
.project-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 16px;
}

.project-card {
  background: #fff;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.08);
  overflow: hidden;
  transition: transform 0.2s ease;
  display: flex;
  flex-direction: column;
  min-height: 320px;
}

.project-card:hover {
  transform: translateY(-4px);
}

.project-banner {
  width: 100%;
  aspect-ratio: 16 / 9;
  object-fit: cover;
  display: block;
}

.project-content {
  padding: 1rem;
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.project-title {
  font-size: 1.05rem;
  font-weight: bold;
  color: #333;
  margin-bottom: 0.5rem;
}

.project-desc {
  font-size: 0.9rem;
  color: #555;
  line-height: 1.4;
  margin-bottom: 0.5rem;
}

.project-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
}

.project-tags span {
  font-size: 0.7rem;
  padding: 3px 6px;
  border-radius: 4px;
  background: #f0f0f0;
  color: #333;
}
</style>

<div id="projects-app" class="projects-page">
  <h1>My Projects</h1>
  <div v-if="projects.length === 0">Loading...</div>
  <div class="project-grid">
    <div v-for="project in projects" :key="project.id" class="project-card">
      <a :href="project.link" target="_blank">
        <img v-if="project.cover" :src="project.cover" :alt="project.title" class="project-banner" />
      </a>
      <div class="project-content">
        <div class="project-title">
          <a :href="project.link" target="_blank">{{ project.title }}</a>
        </div>
        <div class="project-desc">{{ project.description }}</div>
        <div class="project-tags">
          <span v-for="tag in project.tags" :key="tag">#{{ tag }}</span>
        </div>
      </div>
    </div>
  </div>
</div>
{% endraw %}
