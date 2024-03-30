<template>
  <el-card class="box-card">
    <!-- Component Button Add -->
    <FormAdd @handleShowForm="handleShowForm" :isShowForm="isShowForm" />
    <!-- Form Input -->
    <el-form v-if="isShowForm" label-position="right" label-width="100px">
      <el-form-item label="Task Name">
        <el-input v-model="taskName" placeholder="Input task name"></el-input>
      </el-form-item>
      <el-form-item label="Level">
        <el-select v-model="value" placeholder="Select level">
          <el-option
            v-for="item in level"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          >
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button
          v-if="taskSelected === null"
          type="success"
          @click="handleAddTask"
          >Submit</el-button
        >
        <el-button v-else type="warning" @click="handleEditTask"
          >Update</el-button
        >
        <el-button type="info" @click="handleCancel">Cancel</el-button>
      </el-form-item>
    </el-form>
  </el-card>
</template>

<script>
import FormAdd from './FormAdd.vue';
import { v4 as uuidv4 } from 'uuid';

export default {
  name: 'formCtl',
  components: {
    FormAdd
  },
  props: {
    isShowForm: { type: Boolean, default: false }, //truyền từ comp cha vào để show form
    taskSelected: { type: Object, default: null }
  },
  data() {
    return {
      taskName: '',
      level: [
        { value: 0, label: 'Low' },
        { value: 1, label: 'Medium' },
        { value: 2, label: 'High' }
      ],
      value: ''
    };
  },
  watch: {
    taskSelected: function (newData, oldData) {
      if (newData !== null) {
        this.taskName = newData.name;
        this.value = newData.level;
      }
      console.log('watch: ', newData, oldData);
    }
  },
  beforeUpdate() {
    //
  },
  methods: {
    //Truyền yêu cầu từ FormAdd.vue ra App.vue
    handleShowForm() {
      this.$emit('showForm');
    },
    //Xử lý và truyền dữ liệu new task ra App.vue
    handleAddTask() {
      let objTask = {
        id: uuidv4(),
        name: this.taskName,
        level: this.value
      };
      this.$emit('handleAddTask', objTask);
      this.handleCancel();
    },
    //Xử lý sửa Task
    handleEditTask() {
      let objTaskEdit = {
        id: this.taskSelected.id,
        name: this.taskName,
        level: this.value
      };
      this.$emit('editTask', objTaskEdit);
      this.handleResetData();
    },

    handleCancel() {
      this.$emit('showForm');
      this.handleResetData();
    },
    handleResetData() {
      this.taskName = '';
      this.value = '';
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
  min-height: 33vh;
}

.el-form {
  padding-top: 10px;
}
.el-button {
  justify-content: space-between;
  min-width: 45%;
}
@media (max-width: 768px) {
  .box-card {
    min-height: 10vh;
  }
}
</style>
