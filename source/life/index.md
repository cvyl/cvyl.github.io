---
title: "Life Feed"
description: "Webpage displaying Mikka's Feed"
comment: false
---

<!-- Click [here](feed.cvyl.me) to view the webpage in full size. Below is temporary. -->
{% raw %}

  <!-- Import Libraries -->
  <script src="https://unpkg.com/vue@3"></script>
  <script src="https://unpkg.com/tg-blog"></script>
  <link rel="stylesheet" href="https://unpkg.com/tg-blog/dist/style.css">

<!-- Styles & Patches -->
<style>
    #tg-blog-app { font-family: Avenir, Helvetica, Arial, sans-serif }

    /* Icalm Fix: Override img max-width: 100% set in layout.styl */
    #tg-blog-app img { max-width: unset; }

    /* Icalm Fix: overflow-x: hidden breaks infinite scroll */
    .container { overflow-x: unset !important; }
    body { overflow-x: unset !important; }
    .post, .tgb-card, .search {
          background: #fff0ff !important;
    }
    .id, .date, .search, input, ::placeholder {
          color: #f7bdeb !important;
    }
    .tg-blog {
          color: #755c76 !important;
    }
    .reply-text {
        color:rgb(223, 156, 226) !important;
    }
    .reply-to {
        color:rgb(134, 97, 135) !important;
    }
    .reply {
        background: rgb(171, 99, 173) !important;
    }
    .post a {
        color:rgb(134, 97, 135) !important;
    }
    .detail .file-detail {
        color:rgb(223, 156, 226) !important;
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