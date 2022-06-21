<template>
  <div class="container">
    <div class="text-xl sm:text-3xl font-light">
      <!-- heading start -->
      <h2 class="text-center mt-5">Todo App</h2>
      <!-- input -->

      <div class="d-flex mt-5">
        <input
          type="text"
          v-model="task"
          placeholder="Enter task"
          class="w-100 form-control"
        />
        <button class="btn btn-warning rounded-0" @click="submitTask">
          Sumbit
        </button>
      </div>
    </div>

    <!-- Task Table -->
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col" style="width: 120px">Status</th>
          <th scope="col" class="text-center">delete</th>
          <th scope="col" class="text-center">edit</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span
              :class="{
                'line-through': task.status === 'finished',
              }"
              >{{ task.name }}</span
            >
          </td>
          <td>
            <span
              class="pointer select"
              @click="changeStatus(index)"
              :class="{
                'text-danger': task.status === 'to-do',
                'text-success': task.status === 'finished',
                'text-warning': task.status === 'in-progress',
              }"
            >
              {{ capitalizeFirstChart(task.status) }}
              <!-- {{ task.status }} -->
            </span>
          </td>

          <td class="text-center">
            <div @click="deleteTask(index)">
              <span class="fa fa-trash pointer"></span>
            </div>
          </td>

          <td class="text-center">
            <div @click="editTask(index)">
              <p class="fa fa-pen pointer"></p>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "TodoApp",
  props: {
    msg: String,
  },
  data() {
    return {
      task: "",
      editedTask: null,
      statuses: ["to-do", "in-progress", "finished"],

      tasks: [
        {
          name: "Ccl_scrum",
          status: "to-do",
        },
        {
          name: "sepal",
          status: "in-progress",
        },
        {
          name: "Bfs",
          status: "finished",
        },
      ],
    };
  },

  methods: {
    capitalizeFirstChart(str) {
      return str[0].toUpperCase() + str.slice(1);
    },
    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    submitTask() {
      if (this.task.length === 0) return;

      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      } else {
        this.tasks.push({
          name: this.task,
          status: "todo",
        });
      }
      this.task = "";
    },
  },
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
.linethrough {
  text-decoration: line-through;
}
</style>
