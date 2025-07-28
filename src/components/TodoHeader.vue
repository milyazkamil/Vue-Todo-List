<template>
  <div class="row m-0 top-buttons rounded-top p-3 d-flex justify-content-between mb-3">
    <div class='col-12 col-lg-3 mb-3 mb-lg-0'>
      <h2 class='title mb-0' style='line-height: 1.2;'>Vue Todo List</h2>
      <div class='d-flex gap-2' style='font-size: 13px; line-height: 14px;'>
        <small class='title'>Developed by Milyaz Kamil</small>
        <a href="https://github.com/milyazkamil" target="_blank">
          <i class="pi pi-github" style="color: black;"></i>
        </a>
        <a href="https://www.linkedin.com/in/milyazkamil" target="_blank">
          <i class="pi pi-linkedin" style="color: #0077b5;"></i>
        </a>
        <a href="https://www.instagram.com/milyazkamil" target="_blank">
          <i class="pi pi-instagram" style="color: #E1306C;"></i>
        </a>
      </div>
    </div>
    <div class="col-12 col-lg-9">
      <div class="d-flex justify-content-end flex-wrap gap-2">
        <button style='cursor: auto;' class="btn btn-primary d-flex align-items-center rounded-pill p-0 ps-3  fs-6">
          <span class="px-2">All Tasks</span>
          <span class="bg-white text-primary d-flex align-items-center justify-content-center rounded-pill ms-2 px-2 py-1" style="min-width: 30px;">
            {{ totalTasks }}
          </span>
        </button>
        <button style='cursor: auto;' class="btn btn-success d-flex align-items-center rounded-pill p-0 ps-3">
          <span class="px-2">Completed Tasks</span>
          <span class="bg-white text-primary d-flex align-items-center justify-content-center rounded-pill ms-2 px-2 py-1" style="min-width: 30px;">
            {{ completedTasks }}
          </span>
        </button>
        <button :disabled='completedTasks === 0' class="btn btn-outline-danger d-flex align-items-center gap-2 px-3 py-1 rounded-pill" @click="deleteCompletedTasks">
          <i class="pi pi-trash"></i>
          <span>Delete Completed Tasks</span>
        </button>
        <button :disabled='totalTasks === 0' class="btn btn-outline-danger d-flex align-items-center gap-2 px-3 py-1 rounded-pill" @click="deleteAllTasks">
          <i class="pi pi-trash"></i>
          <span>Delete All Tasks</span>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TodoHeader',
  props: {
    totalTasks: {
      type: Number,
      required: true
    },
    completedTasks: {
      type: Number,
      required: true
    }
  },
  methods: {
    deleteCompletedTasks() {
      this.$emit('delete-completed-tasks');
    },
    deleteAllTasks() {
      this.$emit('delete-all-tasks');
    }
  }
}
</script>