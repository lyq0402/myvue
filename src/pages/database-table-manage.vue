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
              <el-breadcrumb-item style="font-size: 20px;"><span style="font-weight: bold; color: #40a9ff">数据库表</span></el-breadcrumb-item>
            </el-breadcrumb>
          </div>

          <div style="margin-top: 20px">
            <el-button type="success" @click="dialogFormVisible = true"  round>新建数据表</el-button>
          </div>

          <el-dialog title="新建数据表" :visible.sync="dialogFormVisible">
            <el-form :model="form">
              <el-form-item label="数据表名" :label-width="formLabelWidth">
                <el-input v-model="form.Name" autocomplete="off"></el-input>
              </el-form-item>
              <el-form-item label="数据表中文名" :label-width="formLabelWidth">
                <el-input v-model="form.ChineseName" autocomplete="off"></el-input>
              </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
              <el-button @click="dialogFormVisible = false">取 消</el-button>
              <el-button type="primary" @click="CreateTable">确 定</el-button>
            </div>
          </el-dialog>

          <div class="table-square-container">
            <!-- 使用 v-for 指令动态生成小方块 -->
            <div class="table-square" v-for="(table, index) in TableList" :key="index" @click="TableDetail(table.tableName)">
              <div class="table-info">
                <div><strong>表格名称:</strong> {{ table.tableName }}</div>
                <div><strong>数据库类型:</strong> {{ table.dataBase }}</div>
                <div><strong>创建日期:</strong> {{ table.createTime }}</div>
              </div>
            </div>
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
  data(){
    return{
      isCollapse:false,
      asideWidth: '200px',
      activeMenu: '/database/manage',
      TableList:["api",
        "api_attribute",
        "api_record",
        "stu_info",
        "stu_info_attribute",
        "table_info"],
      dialogFormVisible: false,
      form: {
        Name: '',
        ChineseName:'',
      },
      formLabelWidth: '120px'
    }
  },
  methods:{
    TableDetail(TableName){
      this.$router.push({ name: 'database-table-manage-index', params: { TableName } });
    },
    CreateTable(){
      axios({
        method: 'post',
        url: 'http://localhost:10010/stuAbility/createChart2',
        data: {
          dimension:this.dimension,
          stuNumber:this.StudentID,
        }
      }).then(response => {
        console.log(response.data)
        this.bar_option.series = response.data.series;
        this.bar_option.xAxis.data = response.data.xAxis;
        this.initializeBarChart();
      }).catch(error => {
        console.log(error);
      })
    },



  },
  mounted() {
    axios.post('http://localhost:10010/DbManage/getTables')
        .then(response => {
          this.TableList = response.data

        })
        .catch(error => {
          console.log(error)
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
.table-info {
  font-size: 18px;
}
.table-square-container {
  display: flex;
  flex-wrap: wrap;
}

.table-square {
  flex: 0 0 calc(33.33% - 20px); /* 每行显示三个方块，可以根据需要调整宽度 */
  padding: 10px;
  margin: 10px;
  border: 1px solid #ccc;
  cursor: pointer;
  text-align: center; /* 文本居中显示 */
  border-radius: 5px; /* 边角弧度 */
  background-color: #def3da; /* 方块底色为浅绿色 */
}

.table-square:hover {
  background-color: #e0e0e0; /* 悬停时的浅色背景色 */
}
</style>