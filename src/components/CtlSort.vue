<template>
  <div class="form-group">
    <div class="dropdown-group">
      <el-dropdown @command="commandSort">
        <el-button type="primary">
          Sort By<i class="el-icon-arrow-down el-icon--right"></i>
        </el-button>
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item
            :command="{ orderBy: 'name', orderDir: 'asc' }"
            icon="el-icon-plus"
            >Name - ASC</el-dropdown-item
          >
          <el-dropdown-item
            :command="{ orderBy: 'name', orderDir: 'desc' }"
            icon="el-icon-circle-plus"
            >Name - DESC</el-dropdown-item
          >
          <el-divider><i class="el-icon-star-on"></i></el-divider>
          <el-dropdown-item
            :command="{ orderBy: 'level', orderDir: 'asc' }"
            icon="el-icon-check"
            >Level - ASC</el-dropdown-item
          >
          <el-dropdown-item
            :command="{ orderBy: 'level', orderDir: 'desc' }"
            icon="el-icon-circle-check"
            >Level - DESC</el-dropdown-item
          >
        </el-dropdown-menu>
      </el-dropdown>
    </div>
    <div class="tag-group">
      <el-tag type="success" effect="dark"
        >{{ orderBy }} - {{ orderDir }}</el-tag
      >
    </div>
  </div>
</template>

<script>
export default {
  name: 'ctlSort',
  data() {
    return {};
  },
  props: {
    orderBy: { type: String, default: 'name' },
    orderDir: { type: String, default: 'asc' }
  },
  methods: {
    //Thu thập input và truyền ra CtlAction.vue
    commandSort(sortBy) {
      this.$emit('commandSort', sortBy);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
* {
  font-family: Arial, Helvetica, sans-serif;
}
.el-button {
  min-width: 150px;
}
.form-group {
  display: flex;
  padding: 10px 0;
}
.tag-group {
  padding: 5px;
  font-weight: 700;
  text-transform: uppercase;
}
</style>
