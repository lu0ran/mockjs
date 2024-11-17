<template>
  <div id="app">
    <!-- <HelloWorld msg="Welcome to Your Vue.js App" />
     -->
    <el-button @click="add">测试添加</el-button>
    <el-table
      :data="tableData"
      :default-sort="{ prop: 'id', order: 'descending' }"
      ref="table"
    >
      <el-table-column prop="id" sortable label="id" width="180">
      </el-table-column>
      <el-table-column prop="name" label="姓名" width="180"> </el-table-column>
      <el-table-column prop="address" label="地址"> </el-table-column>
      <el-table-column prop="date" label="日期"> </el-table-column >
       <el-table-column label="操作"   fixed="right">  <template scope="{row}"> 
        <div>
           <el-button @click="dataDelete(row.id)">删除</el-button>
           <el-button @click="edit(row.id)">修改</el-button>
        </div>
      </template></el-table-column>
     
    </el-table>
  </div>
</template>

<script>
// import HelloWorld from "./components/HelloWorld.vue";
import Mock from "mockjs";
import axios from "axios";
export default {
  name: "App",
  // components: {
  //   HelloWorld,
  // },
  data() {
    return {
      tableData: [],
    };
  },
  created() {
    this.mockData();
    this.getData();
  },
  methods: {
    mockData() {
      Mock.mock("/api/user", "get", {
        code: 200,
        "data|55": [
          {
            id: "@increment",
            name: "@cname()",
            address: "@city(true)",
            date: "@date(yyyy-MM-dd)",
          },
        ],
      });
      Mock.mock(RegExp("/api/user"), "post", (req) => {
        const params = JSON.parse(req.body);
        this.tableData.unshift(
          Mock.mock({
            id: "@increment",
            name: params.name,
            address: params.address,
            date: params.date,
          })
        );
      });
      Mock.mock(RegExp("/api/user"), "delete", (req) => {
        const params = JSON.parse(req.body);
        const deleteIndex = this.tableData.findIndex((item) => {
          return item.id === params;
        });
        this.tableData.splice(deleteIndex, 1);

        return {
          code: 200,
          success: true,
          message: "用户删除成功",
        };
      });
    },
    add() {
      const params = {
        name: "测试",
        address: "测试地址",
        date: "2004-1-1",
      };
      axios.post("/api/user", params).then((res) => {
        console.log(res);
      });
    },
    dataDelete(params) {
      axios.delete("/api/user", params).then((res) => {
           console.log(res);
      });
    },
    edit(){
      axios.updata("/api/user", params).then((res) => {
           console.log(res);
      });
    },
    getData() {
      axios.get("/api/user").then((res) => {
        console.log(res);
        this.tableData = res.data.data;
      });
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
