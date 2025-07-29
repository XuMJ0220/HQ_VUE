<template>
  <div class="note-detail-wrapper">
    <div class="note-detail-container">
      <h1 v-if="note" class="note-title">{{ note.title }}</h1>
      <div v-if="note" class="note-content" v-html="note.contend_html"></div>
      <div v-else>
        <p>加载中...</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import axios from 'axios';

const route = useRoute();
const note = ref(null);

const fetchNote = async () => {
  try {
    const noteId = route.params.id;
    const token = localStorage.getItem('token');
    const response = await axios.get(`/api/v1/admin/notes/${noteId}`,
    {
      headers: { Authorization: `Bearer ${token}` }
    });
    if (response.data.code === 1021) { 
      note.value = response.data.detail;
    } else {
        console.error('获取笔记失败:', response.data.msg);
    }
  } catch (error) {
    console.error('获取笔记详情失败', error);
  }
};

onMounted(() => {
  fetchNote();
});
</script>

<style>
.note-detail-wrapper {
  padding: 2rem;
  color: #e0e0e0;
}

.note-title {
  text-align: center;
  font-size: 2.5em; /* Increased font size */
  margin-bottom: 1rem;
  font-weight: bold;
}

.note-detail-container {
  max-width: 90%;
  margin: 0 auto;
  background: rgba(30, 30, 40, 0.8);
  padding: 2rem;
  border-radius: 15px;
}

.note-content h1 {
  font-size: 2em;
  font-weight: bold;
  margin-top: 0.67em;
  margin-bottom: 0.67em;
}

.note-content h2 {
  font-size: 1.5em;
  font-weight: bold;
  margin-top: 1em;
  margin-bottom: 1em;
}

.note-content h3 {
  font-size: 1.17em;
  font-weight: bold;
  margin-top: 1em;
  margin-bottom: 1em;
}

.note-content p {
  margin-bottom: 1em;
  line-height: 1.6;
}

.note-content ul,
.note-content ol {
  padding-left: 20px;
  margin-bottom: 1em;
}

.note-content ul li {
  margin-bottom: 0.5em;
  list-style-type: disc;
}

.note-content ol li {
  margin-bottom: 0.5em;
  list-style-type: decimal;
}

.note-content pre {
  background-color: #282c34;
  color: #abb2bf;
  padding: 1em;
  border-radius: 5px;
  overflow-x: auto;
  margin-bottom: 1em;
}

.note-content code {
  font-family: 'Courier New', Courier, monospace;
  background-color: #282c34;
  padding: 0.2em 0.4em;
  border-radius: 3px;
}

.note-content pre code {
  background-color: transparent;
  padding: 0;
}

.note-content blockquote {
  border-left: 4px solid #555;
  padding-left: 1em;
  margin-left: 0;
  color: #aaa;
}

.note-content table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 1em;
}

.note-content th,
.note-content td {
  border: 1px solid #555;
  padding: 0.75em;
  text-align: left;
}

.note-content th {
  background-color: #2c2c38;
  font-weight: bold;
  color: #e0e0e0;
}
</style>