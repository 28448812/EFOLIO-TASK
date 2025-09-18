<template>
  <div class="dashboard-container">
    <div class="dashboard-header">
        <h1 class="main-title">Books List</h1>
      <div class="userWrip">
        <div class="username-display">{{ getName() }}</div> 
      <LogoutButton />
    
      </div>
    
    </div>
    <TableComponent />
  </div>
</template>

<script>
import LogoutButton from '@/components/LogoutButton.vue'
import TableComponent from '@/components/dashboard/Table.vue'

export default {
  components: { LogoutButton, TableComponent },
  methods: {
    getName(){
      return localStorage?.getItem('userName')
    }
   }
}
</script>

<style scoped>
.userWrip{
  display: flex;
  gap: 10px;
}

.dashboard-container {
  width: 100%;
  margin: 0 auto;
  padding: 2rem 1.5rem;
  min-height: 100vh;
  background: #f5f5f7;
}

.dashboard-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
  padding-bottom: 1rem;
  border-bottom: 1px solid #e0e0e0;
  gap: 1rem; /* 增加元素间间距，避免拥挤 */
}

.main-title {
  color: #2c3e50;
  font-size: 1.8rem;
  font-weight: 700;
  margin: 0;
}

/* 用户名样式 */
.username-display {
  color: #34495e; /* 与标题颜色协调的深色 */
  font-size: 1.1rem;
  font-weight: 600;
  padding: 0.5rem 1rem;
  background: #ecf0f1; /* 浅灰色背景突出显示 */
  border-radius: 6px; /* 圆角处理 */
  white-space: nowrap; /* 避免用户名换行 */
}
</style>