## 表格页面的模板

```html
<template>
  <div class="app-container">
    <data-table ref="customTable" :request="page" :search-form="searchForm">
      <!-- 搜索插槽BEGIN 搜索条件放到这里 -->
      <template slot="search">
        <el-form-item prop="name">
          <el-input
            v-model="searchForm.name"
            type="text"
            placeholder="请输入人员名称"
          />
        </el-form-item>
      </template>
      <!-- 搜索插槽END -->

      <!-- 按钮插槽BEGIN 需要新增的除了表格默认按钮以外的按钮 -->
      <template slot="function">
        <el-button
          v-permission="['function:admin:user:add']"
          type="primary"
          icon="el-icon-plus"
          @click="handleAdd"
          >新增</el-button
        >
      </template>
      <!-- 按钮插槽END -->

      <template slot="tableColumns">
        <el-table-column
          prop="id"
          label="人员ID"
          :show-overflow-tooltip="true"
          align="center"
        />
        <!-- 过滤器的用法BEGIN，主要用户转换数据类型  例如字典的key转换成相对应name -->
        <el-table-column prop="state" label="单位类型" align="center">
          <template slot-scope="scope">
            <el-tag>
              {{ scope.row.orgType | filterDict(dicts.orgType) }}
            </el-tag>
          </template>
        </el-table-column>
        <!-- 过滤器的用法END -->

        <!-- 操作按钮的用法BEGIN -->
        <el-table-column label="操作" align="center">
          <template slot-scope="scope">
            <el-button-group>
              <el-button
                type="primary"
                size="small"
                @click.native.prevent="edit(scope.row)"
                >修改</el-button
              >
            </el-button-group>
          </template>
        </el-table-column>
        <!-- 操作按钮的用法END -->
      </template>
    </data-table>
    <!-- 引入组件 BEGIN  字典传参 -->
    <Add ref="add" :dicts="dicts" />
    <!-- 引入组件END  -->
  </div>
</template>
<script>
  import DataTable from "@/components/datatable/data-table";
  import Add from "@/所需要引入的地址";
  import { mapGetters } from "vuex";
  export default {
    components: {
      DataTable,
      Add,
    },
    data() {
      return {
        page: page, //table表格list
        searchForm: {
          //查询条件
          orgId: null,
          name: null,
          agentId: null,
        },
      };
    },
    computed: {
      ...mapGetters([
        //获取字典
        "dicts",
      ]),
    },
    created() {
      this.$store.dispatch("getDicts", "orgType"); //设置字典，参数2可以逗号隔开传多个字典key
    },
    methods: {
      // 新增按钮的用法
      handleAdd() {
        this.$refs.add.dialog.showModal = true;
      },
      edit(row) {
        this.$refs.add.dialog.showModal = true;
        this.$refs.add.getInfo(row.id);
      },
    },
  };
</script>
```
