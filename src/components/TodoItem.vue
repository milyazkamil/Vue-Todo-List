<template>
  <li :class="{'bg-success-subtle' : task.completed}" class="list-group-item d-flex justify-content-between gap-4 align-items-center">
    <div class="form-check w-100 d-flex align-items-center gap-3">
      <input class="form-check-input" @change='toggleTaskCompletion(task.id)' type="checkbox" :checked="task.completed">
      <label :class="{'text-decoration-line-through' : task.completed}" class="text-truncate form-check-label" style='max-width: 700px;' v-if="editingTaskId !== task.id">
        {{ editedTitle }}
      </label>
      <div v-if="editingTaskId === task.id" class='d-flex align-items-center gap-3 w-100'>
        <input type="text" class="form-control" value='editedTitle' v-model="editedTitle" @keydown.enter="stopEditing">
      </div>
    </div>
    <div class='d-flex gap-3'>
      <i title='Save Task' @click.stop="stopEditing" v-if="editingTaskId === task.id" class="pi pi-save" style="color: green;"></i>
      <i title='Edit Task' class="pi pi-pen-to-square" style="color: blue; cursor: pointer;" @click="editTask(task.id)" @click.stop="editTask(task.id)"></i>
      <i title='Delete Task' class="pi pi-trash" style="color: red; cursor: pointer;" @click.stop="deleteTask(task.id)"></i>
    </div>
  </li>
</template>

<script>
  export default {
    name: 'TodoItem',
    data() {
      return {
        editedTitle: this.task.title
      };
    },
    props: {
      task: {
        type: Object,
        required: true
      },
      editingTaskId: [String, Number]
    },
    methods: {
      toggleTaskCompletion(id) {
        this.$emit('toggle-task-completion', id);
      },
      editTask(id) {
        this.$emit('edit-task', id);
      },
      stopEditing() {
        this.$emit('stop-editing', {
          id: this.task.id,
          title: this.editedTitle
        });
      },
      deleteTask(id) {
        this.$emit('delete-task', id);
      }
    }
  };
</script>