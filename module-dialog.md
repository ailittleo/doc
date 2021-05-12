### 弹窗的模板

```html
<template>
  <el-dialog
    :title="dialog.title"
    :visible.sync="dialog.showModal"
    :close-on-click-modal="false"
    @close="reset"
  >
    <el-form
      ref="form"
      :model="formData"
      :rules="formRules"
      label-width="120px"
    >
      <!-- 这里存个隐藏的id用于区分是新增还是修改 begin-->
      <el-form-item v-show="false" label="门店id" prop="id">
        <el-input v-model="formData.id" />
      </el-form-item>
      <!-- 这里存个隐藏的id用于区分是新增还是修改 end-->

      <!-- 业务字段begin 具体参见elementui的组件用法 -->
      <el-form-item label="业务员" prop="adminUserId">
        <el-select
          v-model="formData.adminUserId"
          filterable
          placeholder="请选择业务员"
        >
          <el-option
            v-for="item in userList"
            :key="item.id"
            :label="item.name"
            :value="item.id"
          />
        </el-select>
      </el-form-item>
      <!-- 业务字段END 具体参见elementui的组件用法 -->
    </el-form>
    <!-- 底部按钮插槽 BEGIN -->
    <div slot="footer">
      <el-button type="primary" @click="handleSubmit"
        >{{ formData.id?'更新':'保存'}}</el-button
      >
      <el-button type="danger" @click="reset">重置</el-button>
    </div>
    <!-- 底部按钮插槽 END -->
  </el-dialog>
</template>

<script>
  import { store_add, store_update } from "@/api/store"; //导入api模块中api接口
  import { deepClone } from "@/utils"; //导入公共库
  export default {
    props: {
      // 父组件页面传值，这里是字典
      dicts: {
        type: Object,
        default: () => {
          return {};
        },
      },
    },
    data() {
      return {
        //弹窗相关参数
        dialog: {
          showModal: false,
          title: "新增",
        },
        //下拉框绑定list
        userList: [],
        //页面绑定参数
        formData: {
          adminUserId: null,
          shopId: null,
        },
        //页面校验逻辑
        formRules: {
          adminUserId: [
            { required: true, message: "请选择单位名称", trigger: "change" },
          ],
        },
      };
    },
    methods: {
      //获取详情
      getInfo(id) {
        this.formData.shopId = id;
        store_shopQueryAdminUser({}).then((res) => {
          const result = res.data;
          if (result.code === 0) {
            this.userList = result.data;
          }
        });
      },
      //保存或者更新
      handleSubmit() {
        this.$refs.form.validate((valid) => {
          if (valid) {
            const param = deepClone(this.formData);
            //  更新操作
            if (this.formData.id) {
              store_update(param).then((res) => {
                const result = res.data;
                if (result.code === 0) {
                  this.dialog.showModal = false;
                  this.$parent.$refs.customTable.query();
                }
              });
            } else {
              //新增操作
              store_add(param).then((res) => {
                const result = res.data;
                if (result.code === 0) {
                  this.dialog.showModal = false;
                  this.$parent.$refs.customTable.query();
                }
              });
            }
          }
        });
      },
      //**退出需要清空校验**
      reset() {
        this.$refs.form.resetFields();
      },
    },
  };
</script>
```
