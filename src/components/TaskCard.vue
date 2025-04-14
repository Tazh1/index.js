<template>
  <div v-if="!edit" class="task">
    {{ task.text }} <MenuTask class="task-menu" @edit-task="openEditTask" @delete-task="deleteTask"></MenuTask>
  </div>
  <div v-else>
    <div class="input-group">
      <textarea v-model="taskText" ref="edit-task-field" rows="4"></textarea>
      <div class="actions">
        <button class="btn icon-btn btn-danger" @click="closeEdit">
          <i style="font-size: 20px" class="mdi mdi-close"></i>
        </button>
        <button class="btn icon-btn btn-success" @click="saveEdit">
          <i style="font-size: 20px" class="mdi mdi-check"></i>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import MenuTask from './MenuTask.vue'

export default {
  components: {
    MenuTask
  },
  props: {
    task: { type: Object, required: true }
  },
  data() {
    return {
      taskText: this.task.text,
      edit: false
    }
  },
  methods: {
    openEditTask() {
      this.edit = true
      this.$nextTick(() => {
        this.$refs['edit-task-field'].focus()
      })
    },
    closeEdit() {
      this.edit = false
      this.taskText = this.task.text
    },
    saveEdit() {
      if (!this.taskText) return
      this.$emit('edit-task', {
        text: this.taskText,
        taskId: this.task.id,
        columnId: this.task.columnId
      })
      this.closeEdit()
    },
    deleteTask() {
      this.$emit('delete-task', {
        taskId: this.task.id,
        columnId: this.task.columnId
      })
    }
  }
}
</script>

<style>
.task {
  display: flex;
  border: 1px solid rgba(196, 202, 212, 1) !important;
}

.task-menu {
  margin-left: auto;
}
</style>
