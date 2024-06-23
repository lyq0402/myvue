<template>
  <el-container>
    <el-header class="custom-header">
      <div class="header-content">
        <h1 class="header-title">学生学业能力维度观测</h1>
      </div>
      <el-button type="text" class="logout-button" @click="handleLogout">退出</el-button>
    </el-header>


    <el-main>
      <el-row :gutter="10">
        <el-col :span="8">
          <el-card>
            <div class="custom-content">
              <i class="el-icon-trophy"></i>
              我的职业能力(近三年）
            </div>
            <div style="margin-top: 15px; font-size: 15px; font-weight: bold; font-family: '微软雅黑', cursive;">
              选择我的能力
            </div>
          </el-card>
        </el-col>
        <el-col :span="8">
          <el-card>
            aa
          </el-card>
        </el-col>
        <el-col :span="8">
          <el-card>
            aa
          </el-card>
        </el-col>
      </el-row>
    </el-main>
  </el-container>
</template>

<script>
export default {
  methods: {
    handleLogout() {
      // 处理退出逻辑，例如跳转到登录页面或者清空用户信息等
      console.log("Logout clicked");
    }
  }
};
</script>

<style scoped>
.custom-header {
  background-color: #409EFF; /* 自定义背景颜色 */
  color: white; /* 字体颜色 */
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 60px; /* 设置高度 */
  padding: 0 20px; /* 可选：为内容添加一些内边距 */
}

.header-content {
  flex: 1; /* 占据剩余空间 */
  text-align: center; /* 文字居中 */
}

.header-title {
  margin: 0; /* 去除默认的外边距 */
}

.logout-button {
  color: white; /* 将按钮字体颜色设置为白色 */
}

.custom-content {
  font-size: 18px; /* 调整字体大小 */
  color: green;   /* 更改字体颜色 */
}
</style>
