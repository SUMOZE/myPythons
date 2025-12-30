<template>
  <div class="dashboard-container">
    <header class="header">
      <h1>学生管理系统</h1>
      <div class="user-info">
        <span>欢迎，{{ user?.first_name || user?.username }} ({{ user?.role }})</span>
        <el-button @click="handleLogout" type="danger" size="small">退出登录</el-button>
      </div>
    </header>
    
    <div class="content">
      <aside class="sidebar">
        <el-menu
          :default-active="activeMenu"
          class="menu"
          @select="handleMenuSelect"
        >
          <el-menu-item index="students">
            <span>学生管理</span>
          </el-menu-item>
          <el-menu-item index="courses">
            <span>课程管理</span>
          </el-menu-item>
          <el-menu-item index="enrollments">
            <span>选课管理</span>
          </el-menu-item>
        </el-menu>
      </aside>
      
      <main class="main-content">
        <div v-if="activeComponent === 'students'">
          <h2>学生管理</h2>
          <p>这里将展示学生管理功能</p>
        </div>
        <div v-else-if="activeComponent === 'courses'">
          <h2>课程管理</h2>
          <p>这里将展示课程管理功能</p>
        </div>
        <div v-else-if="activeComponent === 'enrollments'">
          <h2>选课管理</h2>
          <p>这里将展示选课管理功能</p>
        </div>
        <div v-else>
          <h2>欢迎使用学生管理系统</h2>
          <p>请选择左侧菜单项以开始操作。</p>
        </div>
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import { ElMessage, ElMessageBox } from 'element-plus'

const router = useRouter()
const user = ref<any>(null)
const activeMenu = ref('dashboard')
const activeComponent = ref('dashboard')

onMounted(() => {
  const userData = localStorage.getItem('user')
  if (userData) {
    user.value = JSON.parse(userData)
  } else {
    router.push('/login')
  }
})

const handleMenuSelect = (key: string) => {
  activeMenu.value = key
  activeComponent.value = key
}

const handleLogout = () => {
  ElMessageBox.confirm(
    '确定要退出登录吗？',
    '提示',
    {
      confirmButtonText: '确定',
      cancelButtonText: '取消',
      type: 'warning',
    }
  ).then(() => {
    localStorage.removeItem('user')
    localStorage.removeItem('token')
    router.push('/login')
    ElMessage.success('已退出登录')
  }).catch(() => {
    // 取消操作
  })
}
</script>

<style scoped>
.dashboard-container {
  height: 100vh;
  display: flex;
  flex-direction: column;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 20px;
  height: 60px;
  background-color: #409eff;
  color: white;
}

.header h1 {
  margin: 0;
}

.user-info {
  display: flex;
  align-items: center;
  gap: 15px;
}

.content {
  display: flex;
  flex: 1;
  overflow: hidden;
}

.sidebar {
  width: 200px;
  background-color: #f5f5f5;
  padding: 20px 0;
}

.menu {
  border-right: none;
}

.main-content {
  flex: 1;
  padding: 20px;
  overflow-y: auto;
}

.main-content h2 {
  margin-top: 0;
}
</style>