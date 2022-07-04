<template>
  <tr>
    <td>{{ info.id }}</td>
    <td>{{ info.bookname }}</td>
    <td>{{ info.author }}</td>
    <td>{{ info.publisher }}</td>
    <td>
      <button @click="del(info.id)">删除</button
      ><button @click="xian(info)">详情</button>
    </td>
  </tr>
</template>

<script>
export default {
  data() {
    return {
      bookObj: {
        // 参数名提前和后台的参数名对上-发送请求就不用再次对接了
        id: "",
        bookname: "",
        author: "",
        publisher: "",
      },
      bookname: "",
    };
  },
  // 设置
  props: {
    // 规定数据类型
    info: {
      type: Object,
      default:() => ({}),
    },
  },
  methods: {
    // 删除
    del(id) {
      this.$emit("delFn", id);
    },
    // 详情
    xian(id) {
      this.$emit("xianFN", id);
      console.log(id);
    },
  },
};
</script>

<style></style>
