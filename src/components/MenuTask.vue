<template>
  <div class="tooltip">
    <svg
      class="tooltip-icon"
      width="20"
      height="20"
      viewBox="0 0 20 20"
      fill="none"
      xmlns="http://www.w3.org/2000/svg"
      @click.stop="tooltip = !tooltip"
    >
      <path
        d="M10 10.8333C10.4602 10.8333 10.8333 10.4602 10.8333 10C10.8333 9.53977 10.4602 9.16667 10 9.16667C9.53976 9.16667 9.16667 9.53977 9.16667 10C9.16667 10.4602 9.53976 10.8333 10 10.8333Z"
        :stroke="tooltip ? '#3D86F4' : '#86949E'"
        stroke-width="2"
        stroke-linecap="round"
        stroke-linejoin="round"
      />
      <path
        d="M15.8333 10.8333C16.2936 10.8333 16.6667 10.4602 16.6667 10C16.6667 9.53977 16.2936 9.16667 15.8333 9.16667C15.3731 9.16667 15 9.53977 15 10C15 10.4602 15.3731 10.8333 15.8333 10.8333Z"
        :stroke="tooltip ? '#3D86F4' : '#86949E'"
        stroke-width="2"
        stroke-linecap="round"
        stroke-linejoin="round"
      />
      <path
        d="M4.16667 10.8333C4.6269 10.8333 5 10.4602 5 10C5 9.53977 4.6269 9.16667 4.16667 9.16667C3.70643 9.16667 3.33333 9.53977 3.33333 10C3.33333 10.4602 3.70643 10.8333 4.16667 10.8333Z"
        :stroke="tooltip ? '#3D86F4' : '#86949E'"
        stroke-width="2"
        stroke-linecap="round"
        stroke-linejoin="round"
      />
    </svg>

    <div v-if="tooltip" class="tooltip-menu">
      <div class="tooltip-item" @click="openEditTask">
        <img src="../assets/edit.svg" />
        Редактировать
      </div>
      <div class="tooltip-item" @click="deleteTask">
        <img src="../assets/delete.svg" />
        Удалить
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tooltip: false
    }
  },
  methods: {
    openEditTask() {
      this.tooltip = false
      this.$emit('edit-task')
    },
    deleteTask() {
      this.tooltip = false
      this.$emit('delete-task')
    }
  }
}
</script>

<style>
.tooltip {
  position: relative;
}

.tooltip-menu {
  position: absolute;
  top: 14px;
  border-radius: 4px;
  border: 1px solid #e3e5e8;
  background-color: white;
  padding-top: 8px;
  padding-bottom: 8px;
  z-index: 10;
}

.tooltip-item {
  display: flex;
  gap: 8px;
  padding: 4px 8px;
  word-break: initial;
}

.tooltip-item:hover {
  background: #e1f1ff;
}
</style>
