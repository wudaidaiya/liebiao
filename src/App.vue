<template>
  <div class="box">
    <input
      class="inp"
      type="text"
      placeholder="搜索-书本名称"
      v-model="sou.bookname"
      @keydown.enter="down"
    />
    <table border="1" width="700" style="border-collapse: collapse" class="tab">
      <thead>
        <tr>
          <th>序号</th>
          <th>书名</th>
          <th>作者</th>
          <th>出版商</th>
          <th>操作</th>
        </tr>
      </thead>
      <tbody>
        <MyDe
          v-for="item in list"
          :key="item.id"
          :info="item"
          @delFn="delFn"
          @xianFN="edetFn"
        ></MyDe>
      </tbody>
    </table>

    <div class="div1">
      <input
        class="inp1"
        type="text"
        placeholder="书名"
        v-model.trim="tian.bookname"
      /><br />
      <input type="inp2" placeholder="作者" v-model.trim="tian.author" /><br />
      <input
        type="inp3"
        placeholder="出版社"
        v-model.trim="tian.publisher"
      /><br />
      <button class="btn" @click="addFn">新增</button>
    </div>
    <div class="div3" v-show="Show">
      <span>id : {{ xiang.id }}</span
      ><br />
      <span>图书名 : {{ xiang.bookname }}</span
      ><br />
      <span>作者 : {{ xiang.author }}</span
      ><br />
      <span>出版社 : {{ xiang.publisher }}</span
      ><br />
    </div>
  </div>
</template>

<script>
import MyDe from "./components/MyDe.vue";
export default {
  data() {
    return {
      list: [],
      sou: { bookname: "" },
      tian: { bookname: "", author: "", publisher: "" },
      xiang: {},
      Show: false,
    };
  },
  created() {
    this.fn();
  },
  methods: {
    //请求数据 渲染列表
    fn() {
      this.$axios({
        url: "/api/getbooks",
      }).then((res) => {
        console.log(res);
        this.list = res.data.data;
        // console.log(this.list);
      });
    },
    //搜索 数据
    down() {
      if (this.sou.bookname.trim().length == 0) return alert("请输入内容");
      this.$axios({
        url: "/api/getbooks",
        params: this.sou,
      }).then((res) => {
        this.list = res.data.data;
      });
    },
    //添加数据
    // async
    addFn() {
      // 1 使用接口的方法
      this.$axios({
        method: "POST",
        url: "/api/addbook",
        data: { ...this.tian },
      }).then(() => {
        // console.log(res);
        // 1.1 获取接口重新渲染页面
        this.fn();
        // console.log(this.list);
      });
      //  清空输入框
      this.tian.author = "";
      this.tian.bookname = "";
      this.tian.publisher = "";
      //2 不使用接口的方法
      //   let id = null;
      //   if (this.list.length === 0) {
      //     id = 4;
      //   } else {
      //     id = this.list[this.list.length - 1].id + 1;
      //   }
      //   const obj = {
      //     id,
      //     ...this.tian,
      //   };
      //   this.list.push(obj);
      // },

      //3 方法一的语法糖 工作中常用try + catch  async + await
      // try {
      //   //try里是执行的代码
      //   const res = await this.$axios({
      //     method: "POST",
      //     url: "/api/addbook",
      //     data: { ...this.tian },
      //   });
      //   // console.log(res);
      //   this.fn();
      // } catch (res) {
      //   //catch返回try代码里执行失败的结果
      //   console.log(res);
      // }
    },
    // 点击删除
    delFn(ele) {
      this.$axios({
        method: "GET",
        url: "/api/delbook",
        params: { id: ele },
      }).then(() => {
        this.fn();
      });
    },
    // 查看详情
    edetFn(val) {
      // 显示div
      this.Show = true;
      this.xiang = val;
    },
  },
  components: {
    MyDe,
  },
};
</script>

<style>
input {
  margin: 20px 0 10px 0;
  background-color: #fff;
  border: 1px solid #000;
  height: 25px;
  width: 160px;
  border-radius: 3px;
}
.div1 {
  margin: 30px 0 0 0;
}
.box {
  margin-left: 20px;
}
.btn {
  margin-top: 30px;
  margin-left: 90px;
  width: 70px;
  height: 26px;
  background-color: rgb(93, 225, 243);
  border: 1px solid #000;
  border-radius: 3px;
}
.div3 {
  border: 3px solid #000;
  width: 500px;
  height: 300px;
  border-radius: 10px;
  margin-top: 30px;
  font-size: 30px;
}
</style>
