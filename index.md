---
layout: default
---

<div id="posts-container">
  {% assign posts = site.posts %}
  {% for post in posts limit: 5 %}
    <div class="post">
      <p class="post-date">{{ post.date | date: "%B %d, %Y" }}</p>
      <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
      <p>{{ post.excerpt }}</p>
      <p><a href="{{ post.url }}">Read more...</a></p>
      <p class="post-tags">tags: <span>{{ post.tags | join: " - " }}</span></p>
    </div>
  {% endfor %}
</div>

<button id="load-more">Load More</button>

<script>
  let currentIndex = 5; // Start with the first 5 posts shown
  const increment = 2; // Number of posts to load each time
  const posts = [
    {% for post in site.posts %}
      {
        url: "{{ post.url }}",
        title: "{{ post.title }}",
        date: "{{ post.date | date: "%B %d, %Y" }}",
        tags: "{{ post.tags | join: ' - ' }}",
        excerpt: "{{ post.excerpt | strip_newlines }}",
      },
    {% endfor %}
  ];

  document.getElementById('load-more').addEventListener('click', function() {
    const postsContainer = document.getElementById('posts-container');

    // Load the next set of posts
    for (let i = currentIndex; i < currentIndex + increment && i < posts.length; i++) {
      const postHTML = `
        <div class="post">
          <p class="post-date">${posts[i].date}</p>
          <h2><a href="${posts[i].url}">${posts[i].title}</a></h2>
          <p>${posts[i].excerpt}</p>
          <p><a href="${posts[i].url}">Read more...</a></p>
          <p class="post-tags">tags: <span>${posts[i].tags}</span></p>
        </div>
      `;
      postsContainer.innerHTML += postHTML;
    }

    currentIndex += increment; // Update currentIndex to reflect all the loaded posts
    
    // Hide the button if all posts are loaded
    if (currentIndex >= posts.length) {
      document.getElementById('load-more').style.display = 'none';
    }
  });
</script>
