<template>
  <el-card class="box-card">
    <CtlSort
      :orderBy="orderBy"
      :orderDir="orderDir"
      @commandSort="commandSort"
    />
    <CtlSearch :strSearch="strSearch" @handleSearch="handleSearch" />
  </el-card>
</template>

<script>
import CtlSearch from './CtlSearch.vue';
import CtlSort from './CtlSort.vue';

export default {
  name: 'ctlAction',
  components: {
    CtlSort,
    CtlSearch
  },

  props: {
    strSearch: { type: String, default: '' }, //Truyền ký tự tìm kiếm từ App.vue vào
    orderBy: { type: String, default: 'name' },
    orderDir: { type: String, default: 'asc' }
  },

  methods: {
    // Gọi sự kiện handleSearch ở comp App.vue và truyền ra data đã nhận được
    handleSearch(data) {
      this.$emit('handleSearch', data);
    },
    //Nhận dữ liệu từ CtlSort.vue và chuyển ra cho App.vue
    commandSort(sortBy) {
      // console.log('Parent', sortBy);
      this.$emit('commandSort', sortBy);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.box-card {
  margin: 5px;
  min-height: 33vh;
}

@media (max-width: 768px) {
  .box-card {
    min-height: 19vh;
  }
}
</style>
