<template>
  <div class='container'>
    <div class="row">
      <div id='todo' class="d-flex justify-content-between flex-column rounded p-0 col-12 col-lg-10 offset-0 offset-lg-1 bg-white border border-2">
        <div class=''>
          <TodoHeader :totalTasks="totalTasks" :completedTasks="completedTasks" @delete-completed-tasks="deleteCompletedTasks" @delete-all-tasks="deleteAllTasks" />
          <div class="task-list px-3">
            <TodoList 
              :tasks="tasks"
              @toggle-task-completion="toggleTaskCompletion" 
              @edit-task="editTask" 
              @stop-editing="stopEditing" 
              @delete-task="deleteTask" 
              :editing-task-id="editingTaskId" 
            />
          </div>
        </div>
        <div style='background-color: #f1f1f1;' class="task-input d-flex align-items-center justify-content-between p-3 gap-5 rounded-bottom">
          <TodoInput v-model="newTaskText" @add-task="addTask" />
          <button class="btn btn-success" @click="addTask" style="min-width: 120px;">Add Task</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import TodoHeader from './components/TodoHeader.vue';
  import TodoInput from './components/TodoInput.vue'
  import TodoList from './components/TodoList.vue'

  export default {
    data() {
      return {
        tasks: [],
        newTaskText: '',
        editingTaskId: null
      };
    },
    components: {
      TodoHeader,
      TodoList,
      TodoInput
    },
    methods: {
      addTask() {
        const newTask = {
          title: this.newTaskText,
          completed: false,
          createdAt: new Date().toISOString()
        }

        if (!newTask.title.trim()) {
          alert('Task title cannot be empty');
          return;
        }

        this.$http.post('https://vue-todo-list-milyaz-default-rtdb.firebaseio.com/tasks.json', newTask)
        .then(response => {
          this.tasks.push({ ...newTask, id: response.data.name });
          this.newTaskText = '';
        })
        .catch(error => {
          console.error(error);
        });
      },
      editTask(taskId) {
        this.editingTaskId = taskId;
      },
      stopEditing() {
        const task = this.tasks.find(task => task.id === this.editingTaskId);
        if (task) {
          this.$http.patch(`https://vue-todo-list-milyaz-default-rtdb.firebaseio.com/tasks/${task.id}.json`, {
            title: task.title
          }).catch(err => console.error(err));
        }
        this.editingTaskId = null;
      },
      deleteTask(taskId) {
        this.$http.delete(`https://vue-todo-list-milyaz-default-rtdb.firebaseio.com/tasks/${taskId}.json`)
        .then(() => {
          this.tasks = this.tasks.filter(task => task.id !== taskId);
        })
        .catch(error => {
          console.error(error);
        });
      },
      deleteCompletedTasks() {
        this.$http.delete('https://vue-todo-list-milyaz-default-rtdb.firebaseio.com/tasks.json?shallow=true')
          .then(() => {
            this.tasks = this.tasks.filter(task => !task.completed);
          })
          .catch(error => {
            console.error(error);
          });
      },
      deleteAllTasks() {
        this.$http.delete('https://vue-todo-list-milyaz-default-rtdb.firebaseio.com/tasks.json')
          .then(() => {
            this.tasks = [];
          })
          .catch(error => {
            console.error(error);
          });
      },
      toggleTaskCompletion(taskId) {
        const task = this.tasks.find(task => task.id === taskId);
        if (task) {
          task.completed = !task.completed;
          this.$http.patch(`https://vue-todo-list-milyaz-default-rtdb.firebaseio.com/tasks/${taskId}.json`, { completed: task.completed })
            .catch(error => {
              console.error(error);
            });
        }
      }
    },
    computed: {
      totalTasks () {
        return this.tasks.length;
      },
      completedTasks () {
        return this.tasks.filter(task => task.completed).length;
      },
    },
    created() {
      this.$http.get('https://vue-todo-list-milyaz-default-rtdb.firebaseio.com/tasks.json')
        .then(res => {
          const data = res.data || {};
          this.tasks = Object
            .entries(data)
            .map(([id, task]) => ({
              id,
              ...task
            }));
        })
        .catch(error => {
          console.error(error);
        });
    },
  }
</script>

<style>
  body {
    display: flex !important;
    align-items: center !important;
    justify-content: center !important;
    height: 100vh !important;
    width: 100% !important;
    background-image: url(../src/assets/bg.svg) !important;
    background-repeat: no-repeat !important;
    background-size: cover !important;
  }

  #todo {
    min-height: 600px;
    height: 60%;
  }

  .list-group-item:hover {
    cursor: pointer;
    background-color: #f8f9fa;
  }

  .list-group {
    max-height: 50vh;
    overflow-y: auto;
  }
</style>