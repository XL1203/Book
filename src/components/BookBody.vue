<template>
  <div class="bookBody">
    <div class="head">
      <ul>
        <li>
          <label>编号:</label>
          <input type="text" v-model="pid" @blur="fn" @focus="fn1" :disabled="idFlag" />
        </li>
        <li>
          <label>名称:</label>
          <input type="text" v-model="pname" :disabled="nameFlag" />
        </li>
        <li>
          <button :disabled="submitFlag" @click="add">确认 / 添加</button>
        </li>
      </ul>
    </div>
    <div class="bodyContet">
      <table class="table">
        <thead class="theader">
          <tr>
            <th>序号</th>
            <th>名称</th>
            <th>时间</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in list" :key="item.id">
            <td>{{item.id}}</td>
            <td>{{item.name}}</td>
            <td>{{item.time}}</td>
            <td>
              <a href="javascript:;" @click.prevent="modify(item.id)">修改</a>|
              <a href="javascript:;" @click.prevent="del(item.id)">删除</a>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  props: ["id", "name", "idFlag", "nameFlag", "submitFlag", "list"],
  data() {
    return {
      pid: this.id,
      pname: this.name,
    };
  },
  methods: {
    modify: function (id) {
      this.$emit("modify", id);
    },
    del: function (id) {
      this.$emit("del", id);
    },
    add: function () {
      this.$emit("add", {
        id: this.pid,
        name: this.pname,
      });
      this.pid = "";
      this.pname = "";
    },
    fn: function () {
      this.$emit("fun", {
        id: this.pid,
        name: this.pname,
        type: "blur",
      });
    },
    fn1: function () {
      this.$emit("fun", {
        id: this.pid,
        name: this.pname,
        type: "focus",
      });
    },
  },
  watch: {
    id: function () {
      this.pid = this.id;
      this.pname = this.name;
    },
  },
};
</script>

<style lang="less" scoped>
.bookBody .head {
  height: 40px;
  line-height: 40px;
  background-color: #f3dcab;
  
  ul li {
    float: left;
    margin: 0 15px;
  }
}

.head::before,
.head::after {
  content: "";
  display: block;
  height: 0;
  visibility: hidden;
  clear: both;
}

.table {
  width: 100%;
  border-collapse: collapse;
}

.table th,
td {
  padding: 0 10px;
  border: 1px dashed #f3dcab;
  height: 35px;
  line-height: 35px;
  background-color: #eee;
}

.table th {
  background-color: #f3dcab;
  border-top: 1px solid grey;
}

[v-cloak] {
  display: none;
}
</style>