---
title: "Life Feed"
description: "Webpage displaying Mikka's Feed"
comment: false
---

<!-- Click [here](feed.cvyl.me) to view the webpage in full size. Below is temporary. -->
{% raw %}

  <!-- Import Libraries -->
  <script src="https://cdn.jsdelivr.net/npm/vue@3"></script>
  <script src="https://cdn.jsdelivr.net/npm/tg-blog"></script>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/tg-blog/dist/style.css">

<!-- Styles & Patches -->
<style>
/* Base fonts & overflow fixes */
#tg-blog-app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
#tg-blog-app img {
  max-width: unset;
}
.container,
body {
  overflow-x: unset !important;
}

/* Properly scoped theming */
.tgb-card {
  background-color: #fff0ff !important;
  color: #755c76 !important;
}

.search {
  background-color: #fff0ff !important;
  color: #f7bdeb !important;
}

.tgb-card .id,
.tgb-card .date,
.search input,
.search ::placeholder {
  color: #f7bdeb !important;
}

.tgb-card .info *,
.tgb-card .reply .mtext {
  color: #f7bdeb !important;
}

.tgb-card .reply::before {
  border: 2px solid rgb(171, 99, 173) !important;
}

.tgb-card .forward,
.tgb-card .forward a,
.tgb-card .reply-to {
  color: rgb(134, 97, 135) !important;
}

.tgb-card .text a {
  position: relative;
  box-shadow: inset 0 -0.1666666667em 0 0 #fff0ff, inset 0 -0.3333333333em 0 0 #f7bdeb;
  color: rgb(134, 97, 135) !important;
  transition: all 0.3s;
  cursor: pointer;
}

.tgb-card .text a:hover {
  color: rgba(134, 97, 135, 0.8) !important;
  box-shadow: unset;
}

.tgb-card .text a::after {
  content: var(--href);
  left: 50%;
  position: absolute;
  min-width: fit-content;
  width: 200px;
  max-width: max-content;
  border-radius: 15px;
  background-color: rgba(171, 99, 173, 0.85);
  backdrop-filter: blur(5px);
  padding: 2px 7px;
  transform: translateX(-50%) translateY(-50%);
  opacity: 0;
  box-shadow: none;
  color: #fff0ff;
  font-size: smaller;
  line-height: 24px;
  word-break: break-word;
  pointer-events: none;
  transition: all 0.3s;
}

.tgb-card .text a:hover::after {
  transform: translateX(-50%) translateY(-100%);
  opacity: 1;
}
</style>

<!-- Template setup (Paste your data url here) -->
<div id="tg-blog-app">
    <tg-blog posts-url="https://raw.githubusercontent.com/cvyl/blog-feed/gh-pages/exports/menhera7/posts.json"></tg-blog>
</div>

<!-- Vue js setup -->
<script>
const app = Vue.createApp().component("tg-blog", TgBlog.TgBlog)
app.mount('#tg-blog-app')

// Hexo patch: Destroy app when page switched
const interval = setInterval(() => {
    if (!document.getElementById('tg-blog-app'))
    {
        app.unmount()
        clearInterval(interval)
    }
}, 1000)
</script>

{% endraw %}