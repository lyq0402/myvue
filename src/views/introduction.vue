<template>
  <div style="box-shadow: 0 0 20px rgba(0,0,0,.3); padding: 20px; border-radius: 10px; margin-bottom: 10px; margin-top:10px; font-size: 16px; font-family: '楷体', cursive;">
    <p>本数据中台目前只针对中台管理人员服务，大致功能为整合学院统一的数据库并为不同部门提供 API 以供接入获取对应的数据。</p>
    <div style="margin-top: 10px;"></div>
    <p>中台主要功能：</p>
    <ul>
      <li>数据查询：可以为中台管理人员方便得获取相应的数据，提供基础的查询功能以及一定的高级查询。</li>
      <li>数据分析：中台管理人员可以在对某一张表的某些数据进行监控其变化，或者在查询基础上对数据进行一定的统计及观察，提供定时报表推送，格式可个性化生成。</li>
      <li>API管理：中台管理人员可以对中台的API接口进行创建以及其他管理，并可以监控API调用情况。可以设置访问频率或者次数阈值等。</li>
      <li>统一库管理：中台管理人员可以连接或断开学院统一库，并可以对统一库中的表数据进行增删，也可以对其中的一些数据进行修改等。</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "introduction"
}
</script>

<style scoped>

</style>