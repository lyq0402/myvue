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
        <el-menu router background-color="#001529" text-color="rgba(255, 255, 255, 0.6)" active-text-color="#ccddcc"
                 style="border:none;"  :default-active = "activeMenu">
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
          <el-menu-item index="/database/manage" >
            <i class="el-icon-coin"></i>
            <span slot="title">统一库管理</span>
          </el-menu-item>
          <el-menu-item index="/ability-prediction">
            <i class="el-icon-s-opportunity"></i>
            <span slot="title">学生学业能力维度观测</span>
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
              <el-breadcrumb-item :to="{ path: '/database/manage' }" style="font-size: 20px;">数据库</el-breadcrumb-item>
              <el-breadcrumb-item :to="{ path: '/database/table/manage' }" style="font-size: 20px;">数据库表</el-breadcrumb-item>
              <el-breadcrumb-item style="font-size: 20px;"><span style="font-weight: bold; color: #40a9ff">数据库表元素</span></el-breadcrumb-item>
            </el-breadcrumb>
          </div>

          <div style="margin-top: 20px">
            <el-select v-model="attribute" placeholder="请选择字段名">
              <el-option
                  v-for="item in dynamicColumns"
                  :key="item.attribute"
                  :label="item.attribute"
                  :value="item.attribute">
              </el-option>
            </el-select>
          </div>

          <el-table :data="pagedData"  class="centered-table" :fit="true" >
            <el-table-column
                stripe
                border
                v-for="(column, index) in dynamicColumns"
                :key="index"
                :prop="column.attribute"
                :label="column.translation"
                :width="getColumnWidth(column.attribute)"
                :resizable="column.resizable"
                :show-overflow-tooltip="true"
            ></el-table-column>
          </el-table>

          <div style="display: flex; align-items: center; justify-content: space-between;">
            <el-pagination
                @size-change="handleSizeChange"
                @current-change="handleCurrentChange"
                :current-page.sync="currentPage"
                :page-sizes="[10, 20, 30, 40]"
                :page-size="pageSize"
                layout="total, sizes, prev, pager, next, jumper"
                :total="tableData.length"
            ></el-pagination>
          </div>

        </el-main>


      </el-container>

    </el-container>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "menu-page",
  props: ['TableName'],
  data(){
    return{
      attribute: '',
      pageSize: 10,
      currentPage: 1,
      pagedData: [],
      isCollapse:false,
      asideWidth: '200px',
      activeMenu: '/database/manage',
      dynamicColumns:[],
      tableData:[],
    }
  },
  methods:{
    getColumnWidth(prop) {
      const maxLength = this.getMaxColumnLength(prop);
      const titleWidth = prop.length * 15; // Assuming each character width is 15px
      const dynamicWidth = Math.max(maxLength * 15, titleWidth);
      return `${Math.min(Math.max(dynamicWidth, 80), 200)}px`; // Ensure the width is between 80px and 200px
    },
    getMaxColumnLength(prop) {
      return this.tableData.reduce((max, item) => {
        const value = String(item[prop] || ''); // Convert property value to string
        return Math.max(max, value.length);
      }, 0);
    },
    handleSizeChange(val) {
      this.pageSize = val;
      this.handleCurrentChange(1); // 切换每页条数时回到第一页
    },
    handleCurrentChange(val) {
      this.currentPage = val;
      const startIndex = (val - 1) * this.pageSize;
      const endIndex = startIndex + this.pageSize;
      this.pagedData = this.tableData.slice(startIndex, endIndex);
    },

  },
  mounted() {
    axios({
      method: 'post',
      url: 'http://localhost:10010/DbManage/getTableData',
      data: {
        tableName: this.TableName,
      }
    }).then(response => {
      console.log(response.data)
      this.dynamicColumns = response.data.mapping;
      this.tableData = response.data.data
      this.pagedData =  this.tableData.slice(0, this.pageSize);

    }).catch(error => {
      console.log(error);
    })
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