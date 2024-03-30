<template>
  <!-- ADD : START -->
  <div class="form-group add-task">
    <el-button v-if="!isShowForm" @click="showAddForm" type="warning"
      >Add New Task <i class="el-icon-circle-plus" />
    </el-button>
    <el-button v-else @click="showAddForm" type="danger">Close Form</el-button>
  </div>
  <!-- ADD : END -->
</template>

<script>
export default {
  name: 'formAdd',

  data() {
    return {};
  },

  props: {
    isShowForm: { type: Boolean, default: false } //Truyền từ comp cha vào để thay đổi nút
  },

  methods: {
    //Truyền request hiển thị form ra FormCtl.vue
    showAddForm() {
      this.$emit('handleShowForm');
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.el-button {
  margin: 10px 0;
  width: 100%;
}
</style>
