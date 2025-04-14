<template>
  <div v-if="visible" class="dialog-overlay">
    <div class="dialog">
      <div class="dialog-content">
        <p>{{ message }}</p>
      </div>
      <div class="dialog-actions">
        <button @click="onConfirm">Удалить</button>
        <button @click="onCancel">Отмена</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DeleteDialog',
  props: {
    visible: {
      type: Boolean,
      required: true
    },
    message: {
      type: String,
      default: 'Вы уверены, что хотите удалить этот элемент?'
    }
  },
  methods: {
    onConfirm() {
      this.$emit('confirm')
    },
    onCancel() {
      this.$emit('cancel')
    }
  }
}
</script>

<style scoped>
.dialog-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.dialog {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  max-width: 400px;
  width: 100%;
}

.dialog-content {
  margin-bottom: 20px;
}

.dialog-actions {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
}

button {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:first-child {
  background: #ff4444;
  color: white;
}

button:last-child {
  background: #ccc;
  color: black;
}
</style>
