<template>
  <div class="admin-wrapper">
    <Particles
      id="tsparticles"
      :particlesInit="particlesInit"
      :particlesLoaded="particlesLoaded"
      :options="particleOptions"
    />
    <div class="admin-container">
      <div class="table-container">
        <div class="table-toolbar">
          <button @click="setupAddModal" class="btn btn-primary">添加分类</button>
        </div>
        <table class="categories-table">
          <thead>
            <tr>
              <th>名称</th>
              <th>操作</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="category in categories" :key="category.ID">
              <td>{{ category.name }}</td>
              <td>
                <div class="action-buttons">
                  <button @click="editCategory(category)" class="btn btn-secondary">编辑</button>
                  <button @click="deleteCategory(category.ID)" class="btn btn-danger">删除</button>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <!-- 添加/编辑分类的模态框 -->
      <div v-if="showAddModal || showEditModal" class="modal-overlay">
        <div class="modal-content">
          <h2 class="modal-title">{{ modalTitle }}</h2>
          <form @submit.prevent="saveCategory" class="modal-form">
            <div class="form-group">
              <label for="categoryName">分类名称</label>
              <input type="text" id="categoryName" v-model="currentCategory.name" required>
            </div>
            <div class="modal-actions">
              <button type="submit" class="btn btn-primary">保存</button>
              <button type="button" @click="closeModal" class="btn btn-tertiary">取消</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import '@/assets/admin.css'; // 引入新的样式文件
import axios from 'axios';
import { loadFull } from "tsparticles";

const categories = ref([]);
const showAddModal = ref(false);
const showEditModal = ref(false);
const currentCategory = ref({ id: null, name: '' });
const modalTitle = ref('');

const particleOptions = {
  background: {
    color: {
      value: 'transparent'
    }
  },
  fpsLimit: 60,
  interactivity: {
    events: {
      onClick: {
        enable: true,
        mode: 'push'
      },
      onHover: {
        enable: true,
        mode: 'repulse'
      },
      resize: true
    },
    modes: {
      bubble: {
        distance: 400,
        duration: 2,
        opacity: 0.8,
        size: 40
      },
      push: {
        quantity: 4
      },
      repulse: {
        distance: 200,
        duration: 0.4
      }
    }
  },
  particles: {
    color: {
      value: '#ffffff'
    },
    links: {
      color: '#ffffff',
      distance: 150,
      enable: true,
      opacity: 0.5,
      width: 1
    },
    collisions: {
      enable: true
    },
    move: {
      direction: 'none',
      enable: true,
      outMode: 'bounce',
      random: false,
      speed: 2,
      straight: false
    },
    number: {
      density: {
        enable: true,
        area: 800
      },
      value: 80
    },
    opacity: {
      value: 0.5
    },
    shape: {
      type: 'circle'
    },
    size: {
      random: true,
      value: 5
    }
  },
  detectRetina: true
};

const fetchCategories = async () => {
  try {
    const token = localStorage.getItem('token');
    const response = await axios.get('/api/v1/admin/categories', {
      headers: { Authorization: `Bearer ${token}` }
    });
    console.log('API Response:', response.data); // Log the full response
    if (response.data.code === 1008) { // CodeQuerySuccess
      categories.value = response.data.detail;
    } else {
      console.error('API returned an error:', response.data); // Log API error response
      categories.value = []; // 清空或处理错误
    }
  } catch (error) {
    console.error('获取分类列表失败:', error.response || error); // Log detailed error
    alert('获取分类列表失败');
  }
};

onMounted(() => {
  fetchCategories();
});

const closeModal = () => {
  showAddModal.value = false;
  showEditModal.value = false;
  currentCategory.value = { id: null, name: '' };
};

const editCategory = (category) => {
  modalTitle.value = '编辑分类';
  currentCategory.value = { ...category };
  showEditModal.value = true;
};

const saveCategory = async () => {
  const token = localStorage.getItem('token');
  const headers = { Authorization: `Bearer ${token}` };
  const payload = { name: currentCategory.value.name };

  try {
    if (showEditModal.value) {
      // 编辑分类
      await axios.put(`/api/v1/admin/categories/${currentCategory.value.ID}`, payload, { headers });
      alert('分类更新成功');
    } else {
      // 添加分类
      await axios.post('/api/v1/admin/categories', payload, { headers });
      alert('分类添加成功');
    }
    closeModal();
    fetchCategories(); // 重新获取列表
  } catch (error) {
    console.error('保存分类失败:', error);
    alert('保存分类失败');
  }
};

const deleteCategory = async (id) => {
  if (!confirm('确定要删除这个分类吗？')) return;

  try {
    const token = localStorage.getItem('token');
    await axios.delete(`/api/v1/admin/categories/${id}`, {
      headers: { Authorization: `Bearer ${token}` }
    });
    alert('分类删除成功');
    fetchCategories(); // 重新获取列表
  } catch (error) {
    console.error('删除分类失败:', error);
    alert('删除分类失败');
  }
};

// 监听添加按钮点击，设置模态框标题
const setupAddModal = () => {
  modalTitle.value = '添加分类';
  currentCategory.value = { id: null, name: '' };
  showAddModal.value = true;
};

const particlesInit = async (engine) => {
  await loadFull(engine);
};

const particlesLoaded = async (container) => {
  console.log("Particles container loaded", container);
};
</script>

<style scoped>
.admin-wrapper {
  position: relative;
  min-height: 100vh;
  overflow: hidden;
}

#tsparticles {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  z-index: 0;
}

.admin-container {
  position: relative;
  z-index: 1;
  max-width: 1200px;
  margin: 2rem auto;
  padding: 2rem;
  background: rgba(25, 25, 25, 0.85);
  border-radius: 15px;
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
  backdrop-filter: blur(10px);
  color: #e0e0e0;
}

h1 {
  color: #fff;
  text-align: center;
  margin-bottom: 2rem;
  font-size: 2.5rem;
}

.actions {
  margin-bottom: 1.5rem;
}

.add-btn {
  padding: 0.75rem 1.5rem;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.3s;
}

.add-btn:hover {
  background-color: #45a049;
}

.categories-table {
  width: 100%;
  border-collapse: collapse;
  background-color: white;
  box-shadow: 0 2px 15px rgba(0,0,0,0.1);
  border-radius: 8px;
  overflow: hidden;
}

.categories-table th, .categories-table td {
  padding: 1rem;
  text-align: left;
  border-bottom: 1px solid #ddd;
  color: #333;
}

.categories-table th {
  background-color: #f2f2f2;
  color: #555;
}

.edit-btn, .delete-btn {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-right: 0.5rem;
  transition: background-color 0.3s;
}

.edit-btn {
  background-color: #2196F3;
  color: white;
}

.edit-btn:hover {
  background-color: #1e87dc;
}

.delete-btn {
  background-color: #f44336;
  color: white;
}

.delete-btn:hover {
  background-color: #e33022;
}

/* Modal styles */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background-color: white;
  padding: 2rem;
  border-radius: 8px;
  width: 90%;
  max-width: 500px;
  box-shadow: 0 5px 15px rgba(0,0,0,0.3);
  color: #333;
}

.modal-content h2 {
  margin-top: 0;
  margin-bottom: 1.5rem;
}

.form-group {
  margin-bottom: 1rem;
}

.form-group label {
  display: block;
  margin-bottom: 0.5rem;
}

.form-group input {
  width: 100%;
  padding: 0.75rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.modal-actions {
  text-align: right;
  margin-top: 1.5rem;
}

.save-btn, .cancel-btn {
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.save-btn {
  background-color: #4CAF50;
  color: white;
  margin-right: 0.5rem;
}

.cancel-btn {
  background-color: #f2f2f2;
  color: #333;
}
</style>