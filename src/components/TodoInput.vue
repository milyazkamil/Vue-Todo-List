<template>
  <input
    type="text"
    class="form-control"
    :value="value"
    @input="$emit('input', $event.target.value)"
    @keydown.enter="handleAddTask"
    placeholder="Add a new task..."
  />
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      default: ''
    }
  },
  methods: {
    handleAddTask() {
      this.$emit('add-task');
    }
  }
};
</script>
