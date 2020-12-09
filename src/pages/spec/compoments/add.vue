<template>
  <div>
    <el-dialog :title="info.title" :visible.sync="info.isShow">
      <el-form :model="form">
        <el-form-item label="规格名称" :label-width="width">
          <el-input v-model="form.specsname" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item
          label="规格属性"
          :label-width="width"
          v-for="(item, index) in arrAttr"
          :key="item.index">
          <el-row>
            <el-col :span="16">
              <el-input v-model="item.value" autocomplete="off"></el-input>
            </el-col>
            <el-col :span="4">
              <el-button type="primary" v-if="index == 0" @click="addAttr"
                >新增规格属性</el-button
              >
              <el-button type="danger" v-else @click="delAttr(index)"
                >删除</el-button
              >
            </el-col>
          </el-row>
        </el-form-item>

        <el-form-item label="状态" :label-width="width">
          <el-switch
            v-model="form.status"
            active-color="#13ce66"
            inactive-color="#ff4949"
            :active-value="1"
            :inactive-value="2"
          >
          </el-switch>
        </el-form-item>
      </el-form>

      <div slot="footer" class="dialog-footer">
        <el-button @click="info.isShow = false">取 消</el-button>
        <el-button type="primary" @click="add" v-if="info.isAdd == true"
          >确 定</el-button
        >
        <el-button type="primary" @click="update()" v-else>修 改</el-button>
      </div>
    </el-dialog>
  </div>
</template>
<script>
import { mapActions, mapGetters } from "vuex";
import { reqspecsAdd,reqspecsListOne } from "../../../util/request";
export default {
  props: ["info"],
  computed: {
    ...mapGetters({
      //cateList: "cate/list",
    }),
  },
  components: {},
  data() {
    return {
      width: "160px",
      //isShow: true,
      form: {
        specsname: "",
        attrs: "",
        status: 1,
      },
      arrAttr: [
        {
          value: "",
        },
      ],
    };
  },
  methods: {
    //重置
    empty() {
      this.form = {
        specsname: "",
        attrs: "",
        status: 1,
      };
    },
    //隐藏弹框
    hide() {
      this.info.isShow = false;
    },
    //添加
    add() {
    this.form.attrs=JSON.stringify(this.arrAttr.map(item=>{return item.value}));
      reqspecsAdd(this.form).then((res) => {
        this.empty();
        this.hide();
      });
    },
    //新增属性
    addAttr() {
      this.arrAttr.push({
        value: "",
      });
    },
    //删除属性
    delAttr(index) {
      this.arrAttr.splice(index, 1);
    },

    ...mapActions({
      requestcateList: "cate/requestcateList",
    }),
    //获取一条数据
    look(id) {
      reqspecsListOne({ id: id }).then((res) => {
        this.form = res.data.list;
        this.form.id = id;
        console.log(this.form);
      });
    },
    //修改
    update() {
      reqcateFdit(this.form).then((res) => {
        this.requestcateList();
        this.hide();
      });
    },
  },
  mounted() {
    this.requestcateList();
  },
};
</script>
<style scoped>
.avatar-uploader .el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}
.avatar-uploader .el-upload:hover {
  border-color: #409eff;
}
.avatar-uploader-icon {
  border: 2px dashed #ccc;
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  line-height: 178px;
  text-align: center;
}
.avatar {
  width: 178px;
  height: 178px;
  display: block;
}
</style>