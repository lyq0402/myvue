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
                 style="border:none;"  :default-active = "$route.path">
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
          <el-submenu  open index="['/api-use-statistics', '/api-create', '/api-detail']">
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
            <el-breadcrumb-item>API管理</el-breadcrumb-item>
            <el-breadcrumb-item>API创建</el-breadcrumb-item>
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
          <el-form ref="form" :model="apiForm" label-width="30%">
            <el-form-item label="API名称:">
              <el-input style="margin-left: 20px; width: 500px;" v-model="apiForm.name"
                        placeholder="请输入简洁、清晰的API名称"
              @blur="checkAPIName"></el-input>
              <i v-if="APINameExisted && everChecked" class="el-icon-check" style="color: green; font-size: 20px;"></i>
              <i v-if="!APINameExisted && everChecked" class="el-icon-close" style="color: red; font-size: 20px;"></i>
            </el-form-item>
            <el-form-item label="API描述:">
              <el-input type="textarea" v-model="apiForm.desc"
                        placeholder="请输入API的描述，用于详细说明API的功能、参数、返回结果等信息。这个描述可以帮助其他开发者理解和正确使用API。"
                        style="margin-left: 20px; width: 500px; height: 60px;"></el-input>
            </el-form-item>
            <el-form-item label="API描述:">
              <el-input type="textarea" v-model="apiForm.sql" :rows="6"
                        placeholder="请输入具体的SQL语言,对数据库的操作包括查看，返回修改数据等"
                        style="margin-left: 20px; width: 500px; height: 140px;"></el-input>
            </el-form-item>
            <el-form-item label="请求方式:">
              <el-select v-model="apiForm.requestMethod" placeholder="请选择请求方式" style="margin-left: 20px">
                <el-option label="get" value="get"></el-option>
                <el-option label="post" value="post"></el-option>
                <el-option label="delete" value="delete"></el-option>
                <el-option label="put" value="put"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="数据库类型:">
              <el-select v-model="apiForm.databaseType" placeholder="请选择数据库类型" style="margin-left: 20px">
                  <el-option
                      v-for="item in databaseTypeList"
                      :key="item.value"
                      :label="item.label"
                      :value="item.value">
                  </el-option>
                </el-select>
            </el-form-item>
            <el-form-item label="请求头参数（可选）:">
              <el-input style="margin-left: 20px; width: 500px;" v-model="apiForm.requestHeader"
                        placeholder="输入API的请求头参数，例如认证信息、内容类型等" ></el-input>
            </el-form-item>
            <el-form-item label="身份验证信息（可选）:">
              <el-input style="margin-left: 20px; width: 500px;" v-model="apiForm.authentication"
                        placeholder="输入身份验证信息，例如用户名、密码、API密钥等" ></el-input>
            </el-form-item>
            <el-form-item label="超时终止（可选）:">
              <el-input-number style="margin-left: 20px;" v-model="apiForm.timeout"
                               :min="0" :max="300" :step="1" step-strictly></el-input-number> ms
            </el-form-item>
            <el-form-item label="业务用户:">
              <el-select style="margin-left: 20px" v-model="user" multiple placeholder="请选择">
                <el-option
                    v-for="item in userList"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value">
                </el-option>
              </el-select>
            </el-form-item>

            <el-form-item style="margin-left: 15% ">
              <el-button type="primary" @click="onSubmit">立即创建</el-button>
              <el-button type="danger" @click="onSubmit">取消</el-button>
            </el-form-item>
          </el-form>
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
      APINameExisted: false,
      everChecked: false,
      isCollapse:false,
      asideWidth: '200px',
      apiForm: {
        sql: '',
        name: '',
        requestMethod: '',
        databaseType: '',
        userid: '',
        url: '',
        requestHeader: '',
        authentication: '',
        timeout: 0,
        user: '',
        desc: ''
      },
      userList: [
        {
          value: '1',
          label: '张三'
        },
        {
          value: '2',
          label: '李四'
        },
        {
          value: '3',
          label: '王五'
        },
        {
          value: '4',
          label: '赵六'
        }
      ],
      user: [],
      databaseTypeList: [
        {
          value: 'mysql',
          label: 'mysql'
        },
        {
          value: 'oracle',
          label: 'oracle'
        },
        {
          value: 'sqlserver',
          label: 'sqlserver'
        },
        {
          value: 'postgresql',
          label: 'postgresql'
        },
        {
          value: 'mongodb',
          label: 'mongodb'
        },
        {
          value: 'redis',
          label: 'redis'
        },
        {
          value: 'elasticsearch',
          label: 'elasticsearch'
        },
        {
          value: 'hive',
          label: 'hive'
        },
        {
          value: 'hbase',
          label: 'hbase'
        },
        {
          value: 'kafka',
          label: 'kafka'
        },
        {
          value: 'flink',
          label: 'flink'
        },
        {
          value: 'spark',
          label: 'spark'
        },
        {
          value: 'hadoop',
          label: 'hadoop'
        },
        {
          value: 'hive',
          label: 'hive'
        },
        {
          value: 'hbase',
          label: 'hbase'
        },
        {
          value: 'kafka',
          label: 'kafka'
        }]
    }
  },
  methods:{
    onSubmit() {
      console.log('submit!');
    },
    checkAPIName(){
      this.everChecked = true;
      axios({
        method: 'post',
        url: 'http://localhost:10010/api/checkname',
        data: {
          name: this.apiForm.name
        }
      }).then(response => {
        this.APINameExisted = response.data

      }).catch(error => {
        console.log(error);
      })
    }
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