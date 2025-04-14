<template>
  <div class="card">
    <div :class="`card-title ${column.bgColor}`">{{ column.title }}</div>
    <div class="card-body">
      <draggable
        v-model="localTasks"
        group="`tasks`"
        item-key="id"
        ghost-class="sortable-ghost"
        drag-class="sortable-drag"
        class="group-dnd"
        @change="onTaskMove"
        draggable=".task"
      >
        <TaskCard
          v-for="task in localTasks"
          :key="task.id"
          :task="task"
          @edit-task="editTask"
          @delete-task="deleteTask"
        />
        <AddTask :column-id="column.id" @add-task="addTask" />
      </draggable>
    </div>
  </div>
</template>

<script>
import AddTask from './AddTask.vue'
import TaskCard from './TaskCard.vue'
import draggable from 'vuedraggable'

export default {
  name: 'ColumnCard',
  components: {
    AddTask,
    draggable,
    TaskCard
  },
  props: {
    column: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      localTasks: this.column.tasks
    }
  },
  methods: {
    addTask(data) {
      this.$emit('add-task', data)
    },
    onTaskMove(event) {
      this.$emit('task-moved', {
        columnId: this.column.id,
        event
      })
    },
    editTask(data) {
      this.$emit('edit-task', data)
    },
    deleteTask(data) {
      this.$emit('delete-task', data)
    }
  }
}
</script>
<style>
.card {
  border: 1px solid rgba(196, 202, 212, 1) !important;
}
.group-dnd {
  height: 80vh;
}
.sortable-ghost {
  opacity: 0;
  background: #c8ebfb;
}
.sortable-drag {
  opacity: 1;
  background: #fff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  transform: rotate(2deg);
}
</style>
