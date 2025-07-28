<template>
  <ul class="list-group">
    <TodoItem 
      v-for="task in tasks"
      :key="task.id"
      :task="task"
      :editing-task-id="editingTaskId"
      @toggle-task-completion="$emit('toggle-task-completion', $event)"
      @edit-task="$emit('edit-task', $event)"
      @stop-editing="$emit('stop-editing', $event)"
      @delete-task="$emit('delete-task', $event)"
    />
  </ul>
</template>
<script>
  import TodoItem from './TodoItem.vue'

  export default {
    name: 'TodoList',
    components: {
      TodoItem
    },
    props: {
      tasks: {
        type: Array,
        required: true
      },
      editingTaskId: [String, Number]
    }
  };
</script>