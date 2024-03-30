<template>
  <el-card class="box-card">
    <div slot="header" class="clearfix">
      <span>LIST TASK</span>
    </div>
    <div class="text item">
      <el-table border :data="taskListData">
        <el-table-column
          align="center"
          width="60"
          prop="index"
          label="ID"
        ></el-table-column>
        <el-table-column
          header-align="center"
          prop="name"
          label="Name"
        ></el-table-column>
        <el-table-column align="center" width="100" label="Level">
          <template slot-scope="scope">
            <el-tag :type="getTagType(scope.row.level)">{{
              getLevelName(scope.row.level)
            }}</el-tag>
          </template>
        </el-table-column>
        <el-table-column align="center" width="160" label="Operations">
          <template slot-scope="scope">
            <el-button size="mini" @click="handleEdit(scope.row)"
              >Edit</el-button
            >
            <el-button
              size="mini"
              type="danger"
              @click="handleDelete(scope.row)"
              >Delete</el-button
            >
          </template>
        </el-table-column>
      </el-table>
    </div>
  </el-card>
</template>

<script>
import mapLevel from '../mocks/level.js';

export default {
  name: 'tdlTable',
  components: {},

  data() {
    return {
      mapLevel: mapLevel
    };
  },

  props: {
    listTask: {
      type: Array,
      default: () => []
    }
  },

  computed: {
    taskListData() {
      return this.listTask.map((task, index) => ({
        ...task,
        index: index + 1
      }));
    }
  },

  methods: {
    //Truyền dữ liệu dòng vào và tiến hành lấy tên cấp độ
    getLevelName(level) {
      return this.mapLevel[level].name;
    },
    //Truyền dữ liệu dòng vào và tiến hành lấy type (infor, warning, danger)
    getTagType(level) {
      return this.mapLevel[level].class;
    },
    //Xác định row cần sửa và gửi ra App.vue
    handleEdit(row) {
      this.$emit('handleEdit', row);
    },
    //Xác định dòng cần xoá và gửi ra App.vue
    handleDelete(row) {
      if (confirm('Do you want to delete task')) {
        this.$emit('handleDelete', row);
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
* {
  font-family: Arial, Helvetica, sans-serif;
}
.box-card {
  margin: 5px;
}
</style>
