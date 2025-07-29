<template>
  <div class="admin-wrapper">
    <div class="admin-container">
      <h1>笔记管理</h1>
      <button @click="showAddNoteModal = true">添加笔记</button>
      <button @click="triggerFileUpload">上传笔记</button>
      <input type="file" ref="fileInput" @change="handleFileUpload" style="display: none" accept=".md" />
      <table>
        <thead>
          <tr>
            <th>标题</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="note in notes" :key="note.id">
            <td>{{ note.title }}</td>
            <td>
              <button @click="viewNote(note)">查看</button>
              <button @click="editNote(note)">编辑</button>
              <button @click="deleteNote(note.id)">删除</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- 添加笔记模态框 -->
    <div v-if="showAddNoteModal" class="modal">
      <div class="modal-content">
        <span class="close" @click="showAddNoteModal = false">&times;</span>
        <h2>添加笔记</h2>
        <input type="text" v-model="newNote.title" placeholder="标题">
        <v-md-editor v-model="newNote.content_md" height="400px"></v-md-editor>
        <button @click="addNote">保存</button>
      </div>
    </div>

    <!-- 编辑笔记模态框 -->
    <div v-if="showEditNoteModal" class="modal">
      <div class="modal-content">
        <span class="close" @click="showEditNoteModal = false">&times;</span>
        <h2>编辑笔记</h2>
        <input type="text" v-model="editingNote.title" placeholder="标题">
        <v-md-editor v-model="editingNote.content_md" height="400px"></v-md-editor>
        <button @click="updateNote">更新</button>
      </div>
    </div>


  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import axios from 'axios';
import VMdEditor from '@kangc/v-md-editor';
import '@kangc/v-md-editor/lib/style/base-editor.css';
import githubTheme from '@kangc/v-md-editor/lib/theme/github.js';
import '@kangc/v-md-editor/lib/theme/style/github.css';

// highlightjs
import hljs from 'highlight.js';

VMdEditor.use(githubTheme, {
  Hljs: hljs,
});

const route = useRoute();
const router = useRouter();
const notes = ref([]);
const categoryId = route.params.categoryId;
const showAddNoteModal = ref(false);
const showEditNoteModal = ref(false);
const showViewNoteModal = ref(false);
const newNote = ref({
  title: '',
  content_md: '',
  category_id: categoryId,
  status: 1
});
const editingNote = ref(null);
const viewingNote = ref(null);
const fileInput = ref(null);

const triggerFileUpload = () => {
  fileInput.value.click();
};

const handleFileUpload = (event) => {
  const file = event.target.files[0];
  if (!file) return;

  const reader = new FileReader();
  reader.onload = async (e) => {
    const content = e.target.result;
    const title = file.name.replace(/\.md$/, ''); // Use filename as title

    const noteToUpload = {
      title: title,
      content_md: content,
      category_id: parseInt(categoryId),
      status: 1
    };

    try {
      const response = await axios.post('/api/v1/admin/notes', noteToUpload, {
        headers: {
          Authorization: `Bearer ${localStorage.getItem('token')}`
        }
      });
      if (response.data.code === 1017) { // CodeCreateSuccess
        alert('笔记上传成功！');
        fetchNotes(); // Refresh the list
      } else {
        alert(`上传失败: ${response.data.msg}`);
      }
    } catch (error) { 
      console.error('上传笔记失败', error);
      alert('上传笔记失败');
    }
  };
  reader.readAsText(file);
  
  // Reset file input to allow uploading the same file again
  event.target.value = ''; 
};

const fetchNotes = async () => {
  try {
    const token = localStorage.getItem('token');

    // 首先获取所有分类
    const categoriesResponse = await axios.get('/api/v1/admin/categories', {
      headers: { Authorization: `Bearer ${token}` }
    });

    let categoryName = '';
    if (categoriesResponse.data.code === 1008) {
      const category = categoriesResponse.data.detail.find(c => c.ID == categoryId);
      if (category) {
        categoryName = category.name;
      }
    }

    // 获取所有笔记
    const notesResponse = await axios.get('/api/v1/admin/notes', {
      headers: { Authorization: `Bearer ${token}` }
    });

    if (notesResponse.data.code === 1019) {
      // 根据 categoryName 过滤笔记
      notes.value = notesResponse.data.detail.filter(note => note.category_name === categoryName);
    } else {
      notes.value = [];
    }
  } catch (error) {
    console.error('获取笔记列表失败', error);
  }
};

const addNote = async () => {
  try {
    const response = await axios.post('/api/v1/admin/notes', newNote.value, {
      headers: {
        Authorization: `Bearer ${localStorage.getItem('token')}`
      }
    });
    if (response.data.code === 1017) {
      fetchNotes();
      showAddNoteModal.value = false;
    }
  } catch (error) {
    console.error('添加笔记失败', error);
  }
};

const viewNote = (note) => {
  router.push({ name: 'note-detail', params: { id: note.id } });
};

const editNote = (note) => {
  editingNote.value = { ...note };
  showEditNoteModal.value = true;
};

const updateNote = async () => {
  try {
    const response = await axios.put(`/api/v1/admin/notes/${editingNote.value.id}`, editingNote.value, {
      headers: {
        Authorization: `Bearer ${localStorage.getItem('token')}`
      }
    });
    if (response.data.code === 1025) {
      fetchNotes();
      showEditNoteModal.value = false;
    }
  } catch (error) {
    console.error('更新笔记失败', error);
  }
};

const deleteNote = async (noteId) => {
  try {
    const response = await axios.delete(`/api/v1/admin/notes/${noteId}`, {
      headers: {
        Authorization: `Bearer ${localStorage.getItem('token')}`
      }
    });
    if (response.data.code === 1015) {
      fetchNotes();
    }
  } catch (error) {
    console.error('删除笔记失败', error);
  }
};

onMounted(() => {
  fetchNotes();
});

</script>

<style scoped>

</style>