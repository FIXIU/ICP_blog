<script setup>
import { ref } from 'vue';
import { my_project_backend } from 'declarations/my_project_backend/index';
let blogs = ref([]);

async function handleSubmit(e) {
  e.preventDefault();
  const target = e.target;
  const title = target.querySelector('#title').value;
  const content = target.querySelector('#content').value;
  const tags = target.querySelector('#tags').value;
  const splitTags = tags.split(', ');

  await my_project_backend.add_blog(title, content, splitTags);
  await getBlogs();
}

async function getBlogs() {
  const tempBlogs = await my_project_backend.get_blogs();
  blogs.value = tempBlogs.map((blog) => {
    return {
      ...blog,
      date: blog.date.toString()
    };
  });
}
getBlogs();
</script>

<template>
  <main>
      <img src="/logo2.svg" alt="DFINITY logo" />
      <br />
      <br />
      <form action="#" @submit="handleSubmit">
        <div id="inputContainer">
          <label for="title">Title:</label>
          <input id="title" alt="Title" type="text" />
        </div>
        <div id="inputContainer">
          <label for="content">Content:</label>
          <input id="content" alt="Content" type="text" />
        </div>
        <div id="inputContainer">
          <label for="tags">Tags:</label>
          <input id="tags" alt="Tags" type="text" />
        </div>
        <button type="submit">Post</button>
      </form>
  </main>
</template>
