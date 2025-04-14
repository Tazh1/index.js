<template>
  <div class="alerts-overlay">
    <div v-for="alert in alerts" :key="alert.id" class="alert">
      <div class="alert-type">
        <i style="font-size: 20px" class="mdi mdi-check-circle text-success"></i>
      </div>
      <div class="alert-content">
        <div class="alert-title">{{ alert.title }}</div>
        <div>{{ alert.content }}</div>
      </div>
      <button class="btn icon-btn" @click="deleteAlert(alert.id)">
        <i style="font-size: 20px" class="mdi mdi-close"></i>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AppAlert',
  props: {
    alerts: {
      type: Array,
      default: () => []
    }
  },
  methods: {
    deleteAlert(id) {
      this.$emit('delete-alert', id)
    }
  }
}
</script>

<style scoped>
.alerts-overlay {
  position: absolute;
  bottom: 20px;
  right: 20px;
}
.alert {
  display: flex;
  background: #fff;
  width: 400px;
  min-height: 80px;
  border: 1px solid rgba(227, 229, 232, 1);
  border-left: 8px solid #22c33d;
  border-radius: 4px;
  box-shadow: 0px 8px 8px 0px rgba(0, 0, 0, 0.08);
  margin-top: 8px;
  margin-bottom: 8px;
  position: relative;
}
.alert > div {
  margin-top: 16px;
}
.alert-type {
  display: flex;
  margin-left: 8px;
  margin-right: 8px;
}
.alert-content {
  display: flex;
  flex-direction: column;
  margin-right: 16px;
}
.alert-title {
  margin-bottom: 8px;
  font-weight: bold;
}
.icon-btn {
  height: fit-content;
  position: absolute;
  top: 8px;
  right: 8px;
  color: rgba(134, 148, 158, 1);
}
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.alert {
  animation: fadeIn 0.5s ease-in-out;
}
</style>
