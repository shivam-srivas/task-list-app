<template>
  <div class="container" style="max-width: 600px">
    <!-- Heading -->
    <h2 class="text-center mt-5">Task List App</h2>

    <!-- Input -->
    <div class="d-flex mt-5">
      <input
        type="text"
        v-model="task"
        placeholder="Enter task"
        class="w-100 form-control"
      />
      <button class="btn btn-warning rounded-0" @click="submitTask">
        SUBMIT
      </button>
    </div>

    <!-- Task table -->

    <!-- Main Table -->
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col" style="width: 120px">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{ 'line-through': task.status === 'finished' }">
              {{ task.name }}
            </span>
          </td>
          <td>
            <span
              class="pointer noselect"
              @click="changeStatus(index)"
              :class="{
                'text-danger': task.status === 'to-do',
                ' text-primary': task.status === 'finished',
                'text-secondary': task.status === 'in-progress',
              }"
            >
              {{ capitalizeFirstChar(task.status) }}
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

    <!-- Filtered Data Table  -->
    <div>
      <div class="btn-group ml-3 mt-3 mb-3">
        <button class="btn btn-danger" @click="showTodoTasks">Todo</button>
        <button class="btn btn-secondary" @click="showIpTasks">
          In-progress
        </button>
        <button class="btn btn-primary" @click="showFinishedTasks">
          Finished
        </button>
      </div>
      <table table-bordered mt-5>
        <tbody>
          <tr v-for="(task, index) in filteredTasks" :key="index">
            <td>
              <span>
                {{ task.name }}
              </span>
            </td>
            <td>
              <span
                class="pointer noselect"
                @click="changeStatus(index)"
                :class="{
                  'text-danger': task.status === 'to-do',
                  ' text-primary': task.status === 'finished',
                  'text-secondary': task.status === 'in-progress',
                }"
              >
                {{ capitalizeFirstChar(task.status) }}
              </span>
            </td>
            <td class="text-center">
              <div @click="deleteTask(index)"></div>
            </td>
            <td class="text-center">
              <div @click="editTask(index)"></div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "mytasklists",

  props: {
    msg: String,
  },

  data() {
    return {
      task: "",
      editedTask: null,
      filteredTasks: [],
      statuses: ["to-do", "in-progress", "finished"],

      /* Status could be: 'to-do' / 'in-progress' / 'finished' */
      tasks: [
        {
          name: "Trip To Goa In October.",
          status: "to-do",
        },
        {
          name: "Create YouTube video.",
          status: "finished",
          
        },  
        {
          name: "DSA 5 Ques Daily.",
          status: "to-do",
        },
        {
          name: "Study Full-Stack Dev 8 hours.",
          status: "in-progress",
        },
      
      
      ],
    };
  },

  methods: {
    /**
     * Capitalize first character
     */
    capitalizeFirstChar(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },

    /**
     * Change status of task by index
     */
    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
    },

    /**
     * Deletes task by index
     */
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    /**
     * Edit task
     */
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    showTodoTasks() {
      this.filteredTasks = this.tasks.filter((task) => task.status === "to-do");
    },
    showIpTasks() {
      this.filteredTasks = this.tasks.filter(
        (task) => task.status === "in-progress"
      );
    },
    showFinishedTasks() {
      this.filteredTasks = this.tasks.filter(
        (task) => task.status === "finished"
      );
    },
    /**
     * Add / Update task
     */
    submitTask() {
      if (this.task.length === 0) return;

      /* We need to update the task */
      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      } else {
        /* We need to add new task */
        this.tasks.push({
          name: this.task,
          status: "todo",
        });
      }

      this.task = "";
    },
  },
  computed: {
    todoTasks() {
      return this.tasks.filter((task) => task.status === "to-do");
    },
    inprogressTasks() {
      return this.tasks.filter((task) => task.status === "in-progress");
    },

    completedTasks() {
      return this.tasks.filter((task) => task.status === "finished");
    },
  },
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
.noselect {
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Old versions of Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome, Edge, Opera and Firefox */
}
.line-through {
  text-decoration: line-through;
}
</style>
