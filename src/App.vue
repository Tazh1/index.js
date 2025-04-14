<template>
  <div id="app">
    <ColumnCard
      v-for="column in columns"
      :key="column.id"
      :column="column"
      @add-task="addTask"
      @task-moved="moveTask"
      @edit-task="editTask"
      @delete-task="deleteTask"
    />
    <Alert :alerts="alerts" @delete-alert="deleteAlert" />
  </div>
</template>
<script>
import ColumnCard from './components/ColumnCard.vue'
import Alert from './components/Alert.vue'
import { v4 as uuid } from 'uuid'

export default {
  name: 'App',
  components: {
    ColumnCard,
    Alert
  },
  data: () => ({
    columns: JSON.parse(localStorage.getItem('columns.RIZVAN')) || [
      { id: uuid(), title: 'На согласовании', bgColor: 'secondary', tasks: [] },
      { id: uuid(), title: 'Новые', bgColor: 'info', tasks: [] },
      { id: uuid(), title: 'В процессе', bgColor: 'warning', tasks: [] },
      { id: uuid(), title: 'Готово', bgColor: 'success', tasks: [] },
      { id: uuid(), title: 'Доработать', bgColor: 'danger', tasks: [] }
    ],
    alerts: []
  }),
  watch: {
    columns: {
      handler(data) {
        localStorage.setItem('columns.RIZVAN', JSON.stringify(data))
      },
      deep: true
    }
  },
  methods: {
    addTask(data) {
      const { result: task } = data
      const column = this.columns.find(el => el.id === task.columnId)
      if (column) {
        this.$set(column.tasks, column.tasks.length, task)
        this.addAlert(`Задача создана в «${column.title}»`, task.text)
      }
    },
    moveTask(data) {
      const { columnId, event } = data
      const column = this.columns.find(el => el.id === columnId)

      if (column) {
        if (event.removed) {
          const removedIndex = event.removed.oldIndex

          this.$set(
            column,
            'tasks',
            column.tasks.filter((task, index) => index !== removedIndex)
          )
        }

        if (event.added) {
          const addedTask = event.added.element
          const addedIndex = event.added.newIndex
          this.$set(column, 'tasks', [
            ...column.tasks.slice(0, addedIndex),
            addedTask,
            ...column.tasks.slice(addedIndex)
          ])
          this.addAlert(`Задача перенесена в «${column.title}»`, addedTask.text)
        }
      }
    },
    editTask(data) {
      const { text, columnId, taskId } = data
      const column = this.columns.find(el => el.id === columnId)

      if (column) {
        const taskIndex = column.tasks.findIndex(task => task.id === taskId)

        if (taskIndex !== -1) {
          this.$set(column.tasks[taskIndex], 'text', text)
        }
      }
    },
    deleteTask(data) {
      const { taskId, columnId } = data
      const column = this.columns.find(el => el.id === columnId)
      if (column) {
        let removedTask
        const taskIndex = column.tasks.findIndex(task => task.id === taskId)
        if (taskIndex !== -1) {
          removedTask = column.tasks.splice(taskIndex, 1)[0]
        }
        this.addAlert('Задача удалена', removedTask.text)
      }
    },
    addAlert(title, content) {
      const alert = {
        id: uuid(),
        title,
        content
      }
      this.$set(this.alerts, this.alerts.length, alert)
      setTimeout(() => {
        this.deleteAlert(alert.id)
      }, 12 * 1000)
    },
    deleteAlert(id) {
      const alertIndex = this.alerts.findIndex(alert => alert.id === id)

      if (alertIndex !== -1) {
        this.alerts.splice(alertIndex, 1)
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin-top: 64px;
  display: flex;
  justify-content: space-between;
}
</style>
