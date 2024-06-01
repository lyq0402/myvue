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
            <el-breadcrumb-item>API管理</el-breadcrumb-item>
            <el-breadcrumb-item>API使用情况</el-breadcrumb-item>
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
          <div style="margin-top: 30px">
            <el-row>
              <el-select v-model="apiName" style="margin-left: 20px;
              margin-right: 20px; font-size: 25px;" placeholder="请选择API名称"
              @change="apiNameChange">
                <el-option
                    v-for="item in apiNames"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value">
                </el-option>
              </el-select>
              <el-select v-model="userName" style="margin-left: 20px; margin-right: 20px; font-size: 25px;" placeholder="请选择业务用户">
                <el-option
                    v-for="item in userNames"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value">
                </el-option>
              </el-select>
              <span style="font-size: 20px;" >
                状态：<span :style="{ color: statusColor }">{{status}}</span>
              </span>
              <el-date-picker
                  style="margin-left: 30px"
                  v-model="dateRange"
                  type="daterange"
                  range-separator="至"
                  start-placeholder="开始日期"
                  end-placeholder="结束日期">
              </el-date-picker>
            </el-row>
            <el-row  style="margin-top: 20px;">
              <span style="font-size: 20px">
                功能描述：
              </span>
              <el-input
                  style="margin-top: 10px;"
                  type="textarea"
                  :autosize="{ minRows: 10, maxRows: 15}"
                  v-model="description"
                  readonly>
              </el-input>
            </el-row>
          </div>
          <div>
            <el-row  style="margin-top: 15px; display: flex; align-items: center; justify-content: flex-end;">
              <el-col :span="3" style="font-size: 20px">
                设置流量上限：
              </el-col>
              <el-col :span="5">
                <el-input v-model="streamNumber" width="200" placeholder="请输入内容"></el-input>
              </el-col>
              <el-col  :span="2" style="margin-left: 20px">
                <el-button type="primary" round>确定</el-button>
              </el-col>
              <el-button style="margin-left: 600px;" type="success" round>启用</el-button>
              <el-button type="danger" round>禁用</el-button>
            </el-row>
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
      apiName:'',
      status:'运行',
      statusColor:'#67c23a',
      streamNumber:'',
      userName:'',
      description:'这里填写对应API的功能介绍和需要注意的事项',
      dateRange:[],
      userNames:[
        {
          value: 'user1',
          label: 'user1'
        },
        {
          value: 'user2',
          label: 'user2'
        },
        {
          value: 'user3',
          label: 'user3'
        },
        {
          value: 'user4',
          label: 'user4'
        },
        {
          value: 'user5',
          label: 'user5'
        },
        {
          value: 'user6',
          label: 'user6'
        },
        {
          value: 'user7',
          label: 'user7'
        },
        {
          value: 'user8',
          label: 'user8'
        },
        {
          value: 'user9',
          label: 'user9'
        },
        {
          value: 'user10',
          label: 'user10'
        },
        {
          value: 'user11',
          label: 'user11'
        },
        {
          value: 'user12',
          label: 'user12'
        },
        ],
      apiNames:[
        {
          value: 'api1',
          label: 'api1'
        },
        {
          value: 'api2',
          label: 'api2'
        },
        {
          value: 'api3',
          label: 'api3'
        },
        {
          value: 'api4',
          label: 'api4'
        },
        {
          value: 'api5',
          label: 'api5'
        },
        {
          value: 'api6',
          label: 'api6'
        },
        {
          value: 'api7',
          label: 'api7'
        },
        {
          value: 'api8',
          label: 'api8'
        },
        {
          value: 'api9',
          label: 'api9'
        },
        {
          value: 'api10',
          label: 'api10'
        },
        {
          value: 'api11',
          label: 'api11'
        },
        {
          value: 'api12',
          label: 'api12'
        },
        ]
    }
  },
  methods:{
    apiNameChange(){
      axios({
        method: 'post',
        url: '/api-users',
        data: {
          APIName: this.apiName
        }
      }).then(response => {
        this.userNames = response.data.userNames;
      }).catch(error => {
        console.log(error)
      })
    }

  },
  mounted(){
    axios.get('/table_data')
        .then(response => {
          this.apiNames = response.data.apiNames;
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

</style>