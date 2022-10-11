<!--
 * @Author: DESKTOP-CQREP7P\easy zhou03041516@163.com
 * @Date: 2022-06-28 08:30:35
 * @LastEditors: DESKTOP-CQREP7P\easy zhou03041516@163.com
 * @LastEditTime: 2022-07-04 08:36:36
 * @FilePath: \shoudong\src\views\Home.vue
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->
<template>
  <div class="Home" @click="aaa">
    <div>
      <el-input
        v-model="input"
        id="input"
        ref="input"
        @keyup.enter="enter"
        :autofocus="true"
      />
      <el-button @click="empty">清空</el-button>
      <el-button type="primary" @click="submit">提交</el-button>
    </div>

    <el-table :data="tableData" height="350" stripe border style="width: 100%">
      <el-table-column type="index" width="50" align="center" />
      <el-table-column
        prop="barCode"
        label="镜片号"
        width="180"
        align="center"
      />
      <el-table-column prop="createTime" label="扫码时间" align="center" />
      <el-table-column fixed="right" label="操作" align="center" width="100">
        <template #default="scope">
          <el-button
            link
            type="danger"
            size="small"
            @click="handleClick(scope.row)"
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>
    <div class="total">总条数：{{ tableData.length }}</div>
  </div>
</template>

<script>
import { ElLoading } from "element-plus";
export default {
  name: "",
  components: {
    // [Button.name]: Button // 此处是引入组件名字的方法
  },
  props: {
    // ***: {
    //   type: ***,
    //   default: ***
    // }
  },
  data() {
    return {
      input: "",
      tableData: [],
    };
  },
  watch: {
    //***: {
    //immediate: false,
    // handler: function(v) {
    //console.log(v);
    //}
    //}
  },
  created() {},
  mounted() {},
  methods: {
    enter() {
      if (this.input.length !== 6) {
        this.input = "";
        return;
      }
      if (isNaN(Number(this.input, 10))) {
        this.input = "";
        return;
      }
      var now = new Date();
      var year = now.getFullYear(); //得到年份
      var month = now.getMonth() + 1; //得到月份
      var date = now.getDate(); //得到日期
      var hour = now.getHours(); //得到小时数
      var minute = now.getMinutes(); //得到分钟数
      var second = now.getSeconds(); //得到秒数
      for (let i = 0; i < this.tableData.length; i++) {
        if (this.input === this.tableData[i].barCode) {
          this.input = "";
          return;
        }
      }
      this.tableData.push({
        barCode: this.input,
        createTime:
          year +
          "-" +
          month +
          "-" +
          date +
          " " +
          hour +
          ":" +
          minute +
          ":" +
          second,
      });
      this.input = "";
    },
    empty() {
      this.input = "";
      this.tableData = [];
    },
    async submit() {
      console.log(this.tableData);
      let options = {
        lock: true,
        text: "Loading",
        background: "rgba(0, 0, 0, 0.7)",
      };
      let loadingInstance = ElLoading.service(options);
      for (let i = 0; i < this.tableData.length; i++) {
        // eslint-disable-next-line no-undef
        let res = await frmKuchun.tijiao(this.tableData[i].barCode);
        console.log(res);
      }
      this.$message({
        message: "提交成功",
        type: "success",
      });
      loadingInstance.close();
      this.empty();
    },
    aaa() {
      this.$refs.input.focus();
    },
    handleClick(v) {
      console.log("v: ", v);
      for (let i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].barCode === v.barCode) {
          this.tableData.splice(i, 1);
        }
      }
    },
  },
};
</script>

<style scoped lang="scss">
.Home {
  width: 100%;
  height: 100%;
  background: #f2f6fc;
  padding: 20px;
  box-sizing: border-box;
  .el-input {
    width: 300px;
    margin-right: 10px;
  }
  .el-table {
    margin-top: 10px;
  }
  .total {
    color: #606266;
    margin-top: 10px;
  }
}
</style>
