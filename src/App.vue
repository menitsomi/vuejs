<template>
  <div id="app">
    <el-row>
      <el-col :xs="24" :sm="24"> <PageTitle /> </el-col>
    </el-row>
    <el-row>
      <el-col :xs="24" :sm="12">
        <!-- Component Sort&Search -->
        <CtlAction
          :orderBy="orderBy"
          :orderDir="orderDir"
          :strSearch="strSearch"
          @handleSearch="handleSearch"
          @commandSort="commandSort"
        />
      </el-col>
      <el-col :xs="24" :sm="12">
        <!-- Component Add new task -->
        <FormCtl
          :isShowForm="isShowForm"
          :taskSelected="taskSelected"
          @showForm="showForm"
          @handleAddTask="handleAddTask"
          @editTask="handleEditTask"
        />
      </el-col>
    </el-row>
    <el-row>
      <el-col :xs="24" :sm="24">
        <!-- Component ListTask -->
        <TdlTable
          :listTask="listTaskSort"
          @handleEdit="handleEdit"
          @handleDelete="handleDelete"
        />
      </el-col>
    </el-row>
  </div>
</template>

<script>
import PageTitle from './components/PageTitle.vue';
import CtlAction from './components/CtlAction.vue';
import FormCtl from './components/FormCtl.vue';
import TdlTable from './components/TdlTable.vue';

export default {
  name: 'App',

  components: {
    PageTitle,
    CtlAction,
    FormCtl,
    TdlTable
  },

  data() {
    return {
      listTask: null,
      isShowForm: false, //Show form add tasks
      strSearch: '',
      orderBy: 'name',
      orderDir: 'asc',
      taskSelected: null
    };
  },

  watch: {
    listTask: function (newTask) {
      var taskString = JSON.stringify(newTask);
      localStorage.setItem('tasks', taskString);
    }
  },

  created() {
    //Lấy listData từ local Storage
    let task = localStorage.getItem('tasks');
    if (task !== null) {
      this.listTask = JSON.parse(task);
    } else {
      this.listTask = [];
    }
    console.log(localStorage.getItem('tasks'));
  },

  computed: {
    //Xử lý tìm kiếm
    listTaskSearch() {
      const { strSearch } = this;

      var searchResult = this.listTask.filter(item => {
        return item.name.toLowerCase().includes(strSearch.toLowerCase());
      });
      return searchResult;
    },
    //Xử lý hiển thị sort
    listTaskSort() {
      var sortResult = [...this.listTaskSearch]; //sao chép kết quả từ method tìm kiếm để tránh ảnh hưởng đến mảng gốc
      sortResult.sort(this.handleSort); //sắp xếp ds công việc đã lọc
      return sortResult;
    }
  },

  methods: {
    //Nhận yêu cầu hiển thị form từ FormCtl và xử lý hiển thị form
    showForm() {
      if (this.isShowForm) this.taskSelected = null;
      this.isShowForm = !this.isShowForm;
    },
    //Nhận request tìm kiếm từ comp con và truyền vào biến strSearch
    handleSearch(data) {
      this.strSearch = data;
    },
    //Nhận dữ liệu từ CtlAction.vue và truyền vào biến orderBy & orderDir
    commandSort(sortBy) {
      this.orderBy = sortBy.orderBy;
      this.orderDir = sortBy.orderDir;
    },
    //method sắp xếp dữ liệu
    handleSort(a, b) {
      var numberSort = this.orderDir === 'asc' ? -1 : 1;

      if (a[this.orderBy] < b[this.orderBy]) return numberSort;
      else if (a[this.orderBy] > b[this.orderBy]) return numberSort * -1;
      return 0;
    },
    //Nhận NewTask từ FormCtl.vue và Đưa task mới vào danh sách task
    handleAddTask(newTask) {
      this.listTask.push(newTask);
    },
    //Nhận obj task từ FormCtl.vue và tiến hành update lại dữ liệu mới
    handleEditTask(task) {
      console.log('app.vue edit-task', task);
      //Tìm index tương ứng với taskEdit nằm trong listtask gốc
      let index = this.listTask.findIndex(item => item.id === task.id);
      console.log('index = ', index, task.id);

      if (index !== -1) {
        //Áp dụng splice để xoá và thêm mới giá trị vào listtask
        this.listTask.splice(index, 1, task);
        this.showForm();
      }
    },
    //Nhận data từ tdlTable.vue và tiến hành thực hiện sửa
    handleEdit(editTarget) {
      this.isShowForm = true;
      this.taskSelected = editTarget;
    },
    //Nhận data từ tdlTable.vue và xoá task đã chọn ra khỏi listTask
    handleDelete(deleteTarget) {
      this.listTask = this.listTask.filter(item => item.id !== deleteTarget.id);
    }
  }
};
</script>

<style>
#app {
  padding: 25px 0;
}
</style>
