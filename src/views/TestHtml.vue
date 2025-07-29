<template>
  <div>
    <h1>测试HTML渲染</h1>
    <div v-if="note" v-html="note.contend_html"></div>
    <div v-else>加载中...</div>
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