<template>
  <div>
    <el-container>
      <!--      边栏侧-->
      <el-aside :width = "asideWidth" style=" min-height: 100vh; background-color: #001529;">
        <div style="height: 60px; line-height: 60px; display: flex;
        color: whitesmoke; align-items: center; justify-content: center;">
          <span style="font-size: 20px; font-weight: bold;">
            学院数据平台
          </span>


        </div>
        <el-menu router background-color="#001529" text-color="rgba(255, 255, 255, 0.6)"
                 active-text-color="#ccddcc"
                 style="border:none;"  :default-active="activeMenu">
          <el-menu-item index="/menus">
            <i class="el-icon-s-platform"></i>
            <span slot="title">工作台</span>
          </el-menu-item>
          <el-menu-item index="/data-search">
            <i class="el-icon-s-data"></i>
            <span slot="title">数据查询与分析</span>
          </el-menu-item>
          <el-menu-item index="/chart-page">
            <i class="el-icon-data-line"></i>
            <span slot="title">报表查询</span>
          </el-menu-item>
          <el-submenu open index="['/api-use-statistics', '/api-create', '/api-detail']">
            <template #title>
              <i class="el-icon-files"></i>
              <span>API管理</span>
            </template>
            <el-menu-item index="/api-use-statistics">API使用情况</el-menu-item>
            <el-menu-item index ="/api-create">API创建</el-menu-item>
            <el-menu-item index="/api-detail">API详细信息查询</el-menu-item>
          </el-submenu>
          <el-menu-item index="/database/manage">
            <i class="el-icon-coin"></i>
            <span slot="title">统一库管理</span>
          </el-menu-item>
        </el-menu>

      </el-aside>
      <el-container>
        <!--        头部-->
        <el-header>
          <el-breadcrumb separator="/">
            <el-breadcrumb-item>首页</el-breadcrumb-item>
            <el-breadcrumb-item>统一库管理</el-breadcrumb-item>
          </el-breadcrumb>

          <div style="flex: 1; width: 0;  display: flex; align-items: center; justify-content: flex-end;">
            <el-dropdown placement="bottom">
              <div style="display: flex; align-items: center; cursor:default" >
                <span style="margin-right: 5px;">用户名</span>
                <i class="el-icon-user-solid"></i>
              </div>
              <el-dropdown-menu slot="dropdown">
                <el-dropdown-item>个人信息</el-dropdown-item>
                <el-dropdown-item>修改密码</el-dropdown-item>
                <el-dropdown-item>退出登录</el-dropdown-item>
              </el-dropdown-menu>
            </el-dropdown>

          </div>
        </el-header>
        <!--        主体-->
        <el-main>
          <div style="margin-top: 10px">
            <el-breadcrumb separator-class="el-icon-arrow-right" style="font-family: 微软雅黑;">
              <el-breadcrumb-item style="font-size: 20px;">统一库管理</el-breadcrumb-item>
              <el-breadcrumb-item style="font-size: 20px;"><span style="font-weight: bold; color: #40a9ff">数据库</span></el-breadcrumb-item>
            </el-breadcrumb>
          </div>
          <div style="margin-top: 30px">
            <el-row style="font-family: 'Arial Black';  font-size: 18px;">
              数据库名：
              <el-input
                  placeholder="请输入内容"
                  v-model="searchData"
                  style="width: 300px;">
                <i slot="prefix" class="el-input__icon el-icon-search"></i>
              </el-input>
              <el-switch
                  style="margin-left: 20px"
                  v-model="checkType"
                  active-text="全字匹配"
                  inactive-text="非全字匹配">
              </el-switch>
              <el-button type="primary" style="margin-left: 20px" icon="el-icon-search" circle></el-button>
            </el-row>
          </div>
          <div style="margin-top: 20px">
            <el-button type="success" @click="open" round>新建数据库</el-button>
          </div>
        </el-main>


      </el-container>

    </el-container>
  </div>
</template>

<script>
export default {
  name: "menu-page",
  data(){
    return{
      isCollapse:false,
      asideWidth: '200px',
      activeMenu: '/database/manage',
      searchData: '',
      checkType: false,
    }
  },
  methods:{

  }
}

</script>

<style>
.el-menu--inline .el-menu-item {
  background-color: #000c17  !important;
}
.el-menu-item:hover, .el-submenu__title:hover{
  color: #eeffee !important;
}
.el-submenu__title{
  margin: 4px;
}
.el-menu-item.is-active {
  background-color: #40a9ff !important;
  border-radius: 7px;
  margin: 4px;
}
.el-menu-item{
  margin: 4px;
}
.el-menu--inline{
  background-color: #000c17 !important;
}
.el-header {
  box-shadow: 2px 0 6px rgba(0,21,41,.35);
  display: flex;
  align-items: center;
}
</style>