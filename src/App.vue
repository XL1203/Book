<template>
  <div id="app">
    <book-head></book-head>
    <book-body
      :id="id"
      :name="name"
      :list="books"
      @modify="modify"
      @del="del"
      @add="handle"
      @fun="resolve"
      :id-flag="idFlag"
      :name-flag="nameFlag"
      :submit-flag="submitFlag"
    ></book-body>
  </div>
</template>


<script>
import BookHeader from "./components/BookHeader";
import BookBody from "./components/BookBody";

export default {
  data() {
    return {
      id: "",
      name: "",
      idFlag: false,
      nameFlag: false,
      submitFlag: false,
      books: [
        {
          id: 1,
          name: "三国演义",
          time: "2020-6-9",
        },
        {
          id: 2,
          name: "西游记",
          time: "2020-6-9",
        },
        {
          id: 3,
          name: "红楼梦",
          time: "2020-6-9",
        },
        {
          id: 4,
          name: "水浒传",
          time: "2020-6-9",
        },
        {
          id: 5,
          name: "JavaScript",
          time: "2020-6-9",
        },
        {
          id: 6,
          name: "Vue",
          time: "2020-6-9",
        },
      ],
    };
  },
  methods: {
    handle: function (param) {
      if (this.idFlag) {
        //修改操作
        this.books.some((item) => {
          if (item.id === param.id) {
            item.name = param.name;
          }
        });
        this.idFlag = false;
      } else {
        //添加操作
        var book = {};
        if (param.id.trim() !== "") {
          book.id = param.id;
        } else {
          return false;
        }
        if (param.name.trim() !== "") {
          book.name = param.name;
        } else {
          return false;
        }
        book.time = "2020";
        this.books.push(book);
      }
      //清空表单
      this.id = "";
      this.name = "";
    },
    resolve: function (param) {
      if (param.type == "focus") {
        this.nameFlag = false;
        this.submitFlag = false;
      } else if (param.type == "blur") {
        this.books.some((item) => {
          if (item.id == param.id) {
            this.nameFlag = true;
            this.submitFlag = true;
            return true;
          }
        });
      }
    },
    modify: function (param) {
      //id不可更改
      this.idFlag = true;
      var book = this.books.filter((item) => {
        return item.id == param;
      });
      //将对应的书籍渲染到界面
      this.id = book[0].id;
      this.name = book[0].name;
    },
    del: function (param) {
      var index = this.books.findIndex((item) => {
        return item.id == param;
      });
      //通过索引删除指定的书籍
      this.books.splice(index, 1);
    },
  },
  components: {
    'book-head':BookHeader,
    'book-body': BookBody
  }
};
</script>
<style lang="less">
* {
  padding: 0;
  margin: 0;
}

ul {
  list-style: none;
}

#app {
  width: 600px;
  margin: 100px auto;
  text-align: center;
}
</style>
