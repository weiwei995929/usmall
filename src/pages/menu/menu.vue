<template>
  <div>
    <el-button type="primary" @click="willAdd">添加</el-button>
    <!-- 添加 -->
    <v-add :info="info" ref='one'></v-add>
    <!-- 列表 -->
    <v-list @edit="edit($event)"></v-list>
  </div>
</template>
<script>
import vAdd from "./components/add";
import vList from "./components/list";
export default {
  components: {
    vAdd,
    vList,
  },
  data() {
    return {
      info: {
        isShow: false,
        isAdd:true, //这是为添加按钮增加一个标识 用来判断是添加还是编辑
        title:'菜单添加'
      },
    };
  },
  methods: {
    willAdd() {
      //点击添加
      this.info.isShow = !this.info.isShow;
      this.info.isAdd = true;
      this.info.title = '菜单添加';
      this.$refs.one.empty()

    },
    //编辑
    edit(e) {
      this.info.isShow = true;
      this.info.isAdd = false;
      this.info.title = '菜单编辑'
      console.log(this.$refs.one);
      this.$refs.one.look(e)
    },
    //
  },
  mounted() {},
};
</script>
<style>
</style>