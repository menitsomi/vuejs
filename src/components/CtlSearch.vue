<template>
  <div class="input-group">
    <el-input
      v-model="input"
      :value="strSearch"
      @input="handleSearch"
      placeholder="Search for..."
      clearable
    >
    </el-input>
  </div>
</template>

<script>
export default {
  name: 'ctlSearch',

  data() {
    return {
      input: ''
    };
  },

  props: {
    strSearch: { type: String, default: '' } //Truyền ký tự tìm kiếm từ App.vue vào
  },

  methods: {
    // Gọi sự kiện handleSearch ở comp CtlAction và truyền event
    handleSearch(e) {
      this.$emit('handleSearch', e);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.input-group {
  margin: 10px 0;
}
</style>
