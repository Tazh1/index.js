<template>
  <div>
    <button v-if="!open" class="btn-add-task btn icon-btn btn-primary" @click="handleOpen">
      <span class="mdi mdi-plus" style="font-size: 20px"></span>
      Добавить
    </button>
    <div v-else>
      <div class="input-group"> 
        <textarea v-model="taskText" ref="add-task-field" rows="4" placeholder="Введите текст..."> </textarea>
        <div class="actions">
          <button class="btn icon-btn btn-danger" @click="handleClose">
            <i style="font-size: 20px" class="mdi mdi-close"></i>
          </button>
          <button class="btn icon-btn btn-success" @click="createTask">
            <i style="font-size: 20px" class="mdi mdi-check"></i>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { v4 as uuid } from 'uuid'

export default {
  name: 'AddTask',
  props: {
    columnId: {
      type: String,
      required: true
    }
  },
  data: () => ({
    taskText: '',
    open: false
  }),
  methods: {
    handleOpen() {
      this.open = true
      this.$nextTick(() => {
        this.$refs['add-task-field'].focus()
      })
    },
    handleClose() {
      this.open = false
    },
    createTask() {
      if (!this.taskText) return
      const task = {
        id: uuid(),
        columnId: this.columnId,
        text: this.taskText
      }
      const data = {
        result: task
      }
      this.taskText = ''
      this.handleClose()
      this.$emit('add-task', data)
    }
  }
}
</script>

<style scoped>
.btn-add-task {
  margin: 8px;
}
</style>
