<template>
  <div>
    <div>
      <el-table
          :data="tableData"
          style="width: 100%"
      >
        <el-table-column
            prop="date"
            label="日期"
            sortable
            width="180">
        </el-table-column>
        <el-table-column
            prop="name"
            label="姓名"
            sortable
            width="180">
        </el-table-column>
        <el-table-column
            prop="address"
            sortable
            label="地址"
            :formatter="formatter">
        </el-table-column>
      </el-table>
    </div>
    <div style="margin-top: 20px;">
      <el-table :data="displayedData" class="custom-table" >
        <el-table-column
            v-for="(column, index) in dynamicColumns"
            sortable
            :key="index"
            :prop="column.prop"
            :label="column.label"
        ></el-table-column>

      </el-table>
      <div class="block">
        <el-pagination
            background
            @current-change="handleCurrentChange"
            :current-page.sync="currentPage"
            :page-size="100"
            layout="prev, pager, next, jumper"
            :total="1000">
        </el-pagination>
      </div>
    </div>
    <div>
      <el-table :data="tableData2" style="width: 100%">
        <el-table-column label="操作">
          <!-- 在表头中插入按钮 -->
          <template v-slot:header>
            <div>
              <span>姓名</span>
              <el-button type="primary" size="mini" @click="handleButtonClick">添加</el-button>
            </div>
          </template>
          <!-- 在表格内容中插入其他操作按钮 -->
          <template v-slot="scope">
            <el-button type="text" size="mini" @click="handleEdit(scope.row)">编辑</el-button>
            <el-button type="text" size="mini" @click="handleDelete(scope.row)">删除</el-button>
          </template>
        </el-table-column>
        <el-table-column prop="date" label="日期"></el-table-column>
        <el-table-column prop="name" label="姓名"></el-table-column>
        <el-table-column prop="address" label="地址"></el-table-column>
      </el-table>
    </div>
  </div>

</template>

<script>
export default {
  data(){
    return{
      currentPage: 1,
      pageSize: 3, // 设置默认每页显示数量为 10
      dynamicColumns: [
        {
          prop: 'date',
          label: '日期'
        },
        {
          prop: 'name',
          label: '姓名'
        },
        {
          prop: 'address',
          label: '地址'
        }
      ],
      tableData: [
        {
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        },
        {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1517 弄'
        },
        {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1519 弄'
        },
        {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1516 弄'
        },
        {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1517 弄'
        },
        {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1519 弄'
        },
        {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1516 弄'
        },
        {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1517 弄'
        },
        {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1519 弄'
        },
        {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1516 弄'
        }
      ],
      tableData2: [
        { date: '2024-05-31', name: '张三', address: '北京' },
        { date: '2024-06-01', name: '李四', address: '上海' },
        { date: '2024-06-02', name: '王五', address: '广州' }
      ]
    }
  },
  computed: {
    totalItems() {
      return this.tableData.length; // 计算总条目数
    },
    displayedData() {
      const start = (this.currentPage - 1) * this.pageSize;
      const end = start + this.pageSize;
      return this.tableData.slice(start, end); // 根据当前页码和每页显示数量，计算要显示的数据
    }
  },
  methods: {
    handleCurrentChange(newPage) {
      this.currentPage = newPage; // 处理页码变化
    },
    handleButtonClick() {
      // 在这里处理按钮点击事件，例如添加新行或打开弹窗等操作
      console.log('按钮被点击了');
    },
    handleEdit(row) {
      // 编辑操作
      console.log('编辑行：', row);
    },
    handleDelete(row) {
      // 删除操作
      console.log('删除行：', row);
    },
    formatter(row, column) {
      return row.address;
    }
  }
}
</script>

<style>
  .custom-table {
    border-collapse: collapse; /* 合并边框 */
  border-color: #333; /* 原始线条颜色 */
}

  .custom-table td,
  .custom-table th {
  border: 1px solid; /* 设置单元格的边框 */
}

  .custom-table::before {
  content: '';
  display: table-row;
  border-bottom: 1px solid #333; /* 设置底部边框颜色 */
}
</style>


